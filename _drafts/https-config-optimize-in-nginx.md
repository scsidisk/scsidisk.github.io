Nginx下配置高性能，高安全性的https TLS服务
=======================================

2015-05-08

下文以nginx为例，介绍如何部署一个高性能，高安全性的https服务器。

并附送一个优化出来的openssl编译脚本，可以编译出一个高性能，高安全性的openssl库，您可以直接复制粘贴使用。

此处直接给出实践指导，后续再写文章解释tls协议的这些原理细节。

nginx下https配置的优化点，主要有:

1.  session ticket
2.  session id cache
3.  ocsp stapling
4.  http KeepAlive
5.  ECDHE等ciphersuite优化
6.  openssl 编译优化

### 一， nginx https的配置

先贴一下nginx配置，如下。

是根据[mozilla的权威文档](https://wiki.mozilla.org/Security/Server_Side_TLS#Nginx)
,和[生成工具](https://mozilla.github.io/server-side-tls/ssl-config-generator/)(选择
nginx，intermediate ) 生成的配置为基础，加入session ticket等配置的结果

    tls\_nginx.conf

    +-------+--------------------------------------+
    |       |     server {                         |
    | 1     |         listen 443 ssl;              |
    | 2     |                                      |
    | 3     |         # certs sent to the client i |
    | 4     | n SERVER HELLO are concatenated in s |
    | 5     | sl_certificate                       |
    | 6     |         ssl_certificate /path/to/sig |
    | 7     | ned_cert_plus_intermediates;         |
    | 8     |         ssl_certificate_key /path/to |
    | 9     | /private_key;                        |
    | 10    |         ssl_session_timeout 1d;      |
    | 11    |         ssl_session_cache shared:SSL |
    | 12    | :50m;                                |
    | 13    |                                      |
    | 14    |         ssl_session_ticket_key /etc/ |
    | 15    | nginx/conf.d/tls_session_ticket.key; |
    | 16    |         ssl_session_tickets on;      |
    | 17    |                                      |
    | 18    |         keepalive_timeout 75s;       |
    | 19    |                                      |
    | 20    |         # Diffie-Hellman parameter f |
    | 21    | or DHE ciphersuites, recommended 204 |
    | 22    | 8 bits                               |
    | 23    |         ssl_dhparam /path/to/dhparam |
    | 24    | .pem;                                |
    | 25    |                                      |
    | 26    |         # intermediate configuration |
    | 27    | . tweak to your needs.               |
    | 28    |         ssl_protocols TLSv1 TLSv1.1  |
    | 29    | TLSv1.2;                             |
    | 30    |         ssl_ciphers 'ECDHE-RSA-AES12 |
    | 31    | 8-GCM-SHA256:ECDHE-ECDSA-AES128-GCM- |
    | 32    | SHA256:ECDHE-RSA-AES256-GCM-SHA384:E |
    | 33    | CDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA |
    | 34    | -AES128-GCM-SHA256:DHE-DSS-AES128-GC |
    | 35    | M-SHA256:kEDH+AESGCM:ECDHE-RSA-AES12 |
    | 36    | 8-SHA256:ECDHE-ECDSA-AES128-SHA256:E |
    | 37    | CDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES1 |
    | ```   | 28-SHA:ECDHE-RSA-AES256-SHA384:ECDHE |
    |       | -ECDSA-AES256-SHA384:ECDHE-RSA-AES25 |
    |       | 6-SHA:ECDHE-ECDSA-AES256-SHA:DHE-RSA |
    |       | -AES128-SHA256:DHE-RSA-AES128-SHA:DH |
    |       | E-DSS-AES128-SHA256:DHE-RSA-AES256-S |
    |       | HA256:DHE-DSS-AES256-SHA:DHE-RSA-AES |
    |       | 256-SHA:AES128-GCM-SHA256:AES256-GCM |
    |       | -SHA384:AES128-SHA256:AES256-SHA256: |
    |       | AES128-SHA:AES256-SHA:AES:CAMELLIA:D |
    |       | ES-CBC3-SHA:!aNULL:!eNULL:!EXPORT:!D |
    |       | ES:!RC4:!MD5:!PSK:!aECDH:!EDH-DSS-DE |
    |       | S-CBC3-SHA:!EDH-RSA-DES-CBC3-SHA:!KR |
    |       | B5-DES-CBC3-SHA';                    |
    |       |         ssl_prefer_server_ciphers on |
    |       | ;                                    |
    |       |                                      |
    |       |         # HSTS (ngx_http_headers_mod |
    |       | ule is required) (15768000 seconds = |
    |       |  6 months)                           |
    |       |         add_header Strict-Transport- |
    |       | Security max-age=15768000;           |
    |       |                                      |
    |       |         # OCSP Stapling ---          |
    |       |         # fetch OCSP records from UR |
    |       | L in ssl_certificate and cache them  |
    |       |         ssl_stapling on;             |
    |       |         ssl_stapling_verify on;      |
    |       |                                      |
    |       |         ## verify chain of trust of  |
    |       | OCSP response using Root CA and Inte |
    |       | rmediate certs                       |
    |       |         ssl_trusted_certificate /pat |
    |       | h/to/signed_cert_plus_intermediates; |
    |       |                                      |
    |       |         resolver 114.114.114.114;    |
    |       |                                      |
    |       |         ....                         |
    |       |     }                                |
    +-------+--------------------------------------+


### 二， 其中的几个配置项

##### 1. /path/to/signed\_cert\_plus\_intermediates

是你的证书链，就是ca签署之后发给你的文件，是一串这样的内容

    tls\_nginx.conf

    +-------+--------------------------------------+
    |       |     -----BEGIN CERTIFICATE-----      |
    | 1     |     ...                              |
    | 2     |     -----END CERTIFICATE-----        |
    | 3     |     -----BEGIN CERTIFICATE-----      |
    | 4     |     ...                              |
    | 5     |     -----END CERTIFICATE-----        |
    | 6     |     ...                              |
    | 7     |     -----BEGIN CERTIFICATE-----      |
    | 8     |     ...                              |
    | 9     |     -----END CERTIFICATE-----        |
    | 10    |                                      |
    |       |                                      |
    +-------+--------------------------------------+


##### 2. /path/to/private\_key

是你的私钥，一定要保密，最好chown+chmod限制只有http
server可以读取！内容大致是：

    tls\_nginx.conf


    +------+--------------------------------------+
    |      |     -----BEGIN RSA PRIVATE KEY-----  |
    | 1    |     ...                              |
    | 2    |     -----END RSA PRIVATE KEY-----    |
    | 3    |                                      |
    |      |                                      |
    +------+--------------------------------------+



##### 3. /path/to/dhparam.pem

是dhe密钥协商的参数，生成方法：

    openssl  dhparam -out dhparam.pem 2048

##### 4. /etc/nginx/conf.d/tls\_session\_ticket.key

是用来加解密session
ticket的密码文件，这个文件的安全等级应该和私钥一样，最好chmod+chown限制只有http
server能读取。 生成方法:

    openssl rand 48 > tls_session_ticket.key

最好定期（比如2天一次）轮换，配置文件里把新的key文件放在最前面，旧的在下面，如下

    ssl_session_ticket_key current.key;
    ssl_session_ticket_key previous.key;

##### 5. ssl\_trusted\_certificate /path/to/signed\_cert\_plus\_intermediates

ssl\_trusted\_certificate，是用来验证ocsp响应的各个ca证书+中级证书，和信任的ca根证书列表。当用来验证ocsp响应的时候，应该配置为你的ca根证书+和中级ca证书的列表，此处可以简单和ssl\_certificate使用同一个证书列表文件。\
 其中当需要使用tls的客户端认证的时候（大多数https
server都用不到客户端认证），需要指定信任的ca根证书列表文件,
这个文件在centos里面是/etc/ssl/certs/ca-bundle.trust.crt 这个文件。

配置完成之后，使用ssllabs的这个在线检查工具<https://www.ssllabs.com/ssltest/analyze.html>
做检查，里面提出的警告，都需要改正。

### 三， openssl编译优化

主流的https
server，都是依赖openssl来提供tls协议，openssl本身代码复杂，概念繁多，最近对这方面做了研究，下面是一个深入分析过后的最优化编译配置，可以编译出一个高性能，高安全性的openssl版本，您可以直接复制粘贴使用。

openssl请使用最新的1.0.2a版本，这个版本有intel针对ecdh
p256曲线的优化，编译脚本见本文末尾。

编译好之后，需要让nginx使用我们编译的这个openssl库，在centos下，可以用LD\_LIBRARY\_PATH的办法：

    sudo vim /usr/lib/systemd/system/nginx.service

在其中添加一行Environment，如下：

    [Service]
    ...
    Environment=LD_LIBRARY_PATH=/usr/local/bin/openssl-1.0.2a_64_build/lib/
    ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
    ...

其中的路径，请自行修改。

还需要创建软连接，解决so库版本号的问题，如下

    ln -s libcrypto.so.1.0.0 libcrypto.so.10
    ln -s libssl.so.1.0.0 libssl.so.10

脚本中包含了一个cloudflare的patch，增加chacha20\_poly1305
cipher，使用了这个patch之后，可以使用[cloudflare的ciphers配置](https://github.com/cloudflare/sslconfig/blob/master/conf)

    build\_openssl.sh (build\_openssl.sh)
    [download](/downloads/code/build_openssl.sh)


    +------+--------------------------------------+
    |      |     #!/bin/sh                        |
    | 1    |                                      |
    | 2    |     #openssl优化编译                 |
    | 3    |                                      |
    | 4    |     #查看所有编译选项：              |
    | 5    |     #grep -r '^#if.*OPENSSL_NO' cryp |
    | 6    | to/ ssl/ | grep -o 'OPENSSL_NO_[a-zA |
    | 7    | -Z0-9_]*' | sort -u | sed 's/OPENSSL |
    | 8    | _//' | tr '[A-Z_]' '[a-z-]'          |
    | 9    |                                      |
    | 10   |     #ARCH的选择是看Configure里面的优化选项，选择优化选 |
    | 11   | 项相对更合理的，                     |
    | 12   |     #32 bit linux-elf比较合适，比较糟糕的如 lin |
    | 13   | ux-generic32 就关掉了汇编优化        |
    | 14   |     #64 bit linux-x86_64 比较合适，比较糟糕的如 |
    | 15   |  linux-generic64，也是关掉了汇编优化 |
    | 16   |                                      |
    | 17   |     #逐选项解释：                    |
    | 18   |     #no-shared               \ 不需要.s |
    | 19   | o，只需要.a                          |
    | 20   |     #threads                 \ 必须支持t |
    | 21   | hread，初始化的时候，需要调用CRYPTO_set_locking_ |
    | 22   | callback                             |
    | 23   |     #no-zlib                 \ 压缩，不需 |
    | 24   | 要，有CRIME攻击                      |
    | 25   |     #no-bf                   \ blowf |
    | 26   | ish,废弃                             |
    | 27   |     #no-buf-freelists        \ 不需要，和 |
    | 28   | HeartBleed攻击有关                   |
    | 29   |     #no-camellia             \ camel |
    | 30   | lia，用不到                          |
    | 31   |     #no-cast                 \ cast， |
    | 32   | 废弃                                 |
    | 33   |     #no-comp                 \ comp， |
    | 34   | 压缩，用不到，有CRIME攻击，必须关闭  |
    | 35   |     #no-dtls1                \ dtls， |
    | 36   | 用不到                               |
    | 37   |     #no-decc-init            \ 用不到 |
    | 38   |                                      |
    | 39   |     #no-deprecated           \ 去掉一些废 |
    | 40   | 弃的api                              |
    | 41   |     #no-dsa                  \ dsa ， |
    | 42   | 用不到                               |
    | 43   |     #no-gmp                  \ gmp，用 |
    | 44   | 不到                                 |
    | 45   |     #no-gost                 \ gost， |
    | 46   | 淘汰                                 |
    | 47   |     #no-heartbeats           \ heart |
    | 48   | beats, 用不到，HeartBleed攻击        |
    | 49   |     #no-idea                 \ idea， |
    | 50   | 淘汰                                 |
    | 51   |     #no-jpake                \ jpake |
    | 52   | ，用不到                             |
    | 53   |     #no-krb5                 \ kerbe |
    | 54   | ros认证，用不到                      |
    | 55   |     #no-libunbound           \ 用libu |
    | 56   | nbound做dns resolve，用不到          |
    | 57   |     #no-md2                  \ md2，淘 |
    | 58   | 汰                                   |
    | 59   |     #no-md4                  \ md4，淘 |
    | 60   | 汰                                   |
    | 61   |     #no-mdc2                 \ mdc2， |
    | 62   | 淘汰                                 |
    | 63   |     #no-psk                  \ psk，用 |
    | 64   | 不到                                 |
    | 65   |     #no-rc2                  \ rc2，淘 |
    | 66   | 汰                                   |
    | 67   |     #no-rc5                  \ rc5，淘 |
    | 68   | 汰                                   |
    | 69   |     #no-sctp                 \ sctp， |
    | 70   | 用不到                               |
    | 71   |     #no-seed                 \ seed， |
    | 72   | 用不到                               |
    | 73   |     #no-sha0                 \ sha0， |
    | 74   | 淘汰                                 |
    | 75   |     #no-srp                  \ srp，用 |
    | 76   | 不到                                 |
    | 77   |     #no-srtp                 \ srtp， |
    | 78   | 用不到                               |
    | 79   |     #no-ssl2                 \ ssl2  |
    | 80   | 必须禁用， POODLE攻击                |
    | 81   |     #no-ssl3                 \ ssl3  |
    | 82   | 必须禁用， POODLE攻击                |
    | 83   |     #no-ssl3-method          \ ssl3  |
    | 84   | 必须禁用， POODLE攻击                |
    | 85   |     #no-unit-test            \       |
    | 86   |                                      |
    | 87   |                                      |
    | 88   |     function build(){                |
    | 89   |                                      |
    | 90   |     wget -c https://www.openssl.org/ |
    | 91   | source/$PKG.tar.gz                   |
    | 92   |     wget -c https://raw.githubuserco |
    | 93   | ntent.com/cloudflare/sslconfig/maste |
    | 94   | r/patches/openssl__chacha20_poly1305 |
    | 95   | _cf.patch                            |
    | 96   |                                      |
    | 97   |     #校验一下sha1sum                 |
    | 98   |     RIGHT_SHA1=$(curl -k https://www |
    | 99   | .openssl.org/source/$PKG.tar.gz.sha1 |
    | 100  | )                                    |
    | 101  |     REAL_SHA1=$(sha1sum openssl-1.0. |
    | 102  | 2a.tar.gz   | cut  -f 1 -d ' ' )     |
    | 103  |     if [ "$REAL_SHA1" !=  "$RIGHT_SH |
    | 104  | A1" ] ;  then                        |
    | 105  |         echo "sha1sum check failed!" |
    | 106  |         exit;                        |
    | 107  |     fi                               |
    | 108  |                                      |
    | 109  |     rm -rf $PREFIX                   |
    | 110  |     mkdir -p $PREFIX                 |
    | 111  |                                      |
    | 112  |     rm -rf $PKG                      |
    | 113  |     mkdir -p $PKG                    |
    | 114  |     tar xf $PKG.tar.gz               |
    | 115  |     cd $PKG                          |
    | 116  |                                      |
    | 117  |     patch -p1 < ../openssl__chacha20 |
    | 118  | _poly1305_cf.patch                   |
    | 119  |                                      |
    | 120  |     make dclean                      |
    | 121  |                                      |
    | 122  |     ./Configure                 \    |
    | 123  |         --prefix=$PREFIX        \    |
    | 124  |         $ARCH                   \    |
    | 125  |         threads                 \    |
    | 126  |         shared                  \    |
    | 127  |         no-deprecated           \    |
    | 128  |         no-dynamic-engine       \    |
    | 129  |         no-zlib                 \    |
    | 130  |         no-bf                   \    |
    | 131  |         no-buf-freelists        \    |
    | 132  |         no-cast                 \    |
    | 133  |         no-comp                 \    |
    | 134  |         no-dtls1                \    |
    | 135  |         no-decc-init            \    |
    | 136  |         no-dsa                  \    |
    | 137  |         no-gmp                  \    |
    | 138  |         no-gost                 \    |
    |      |         no-heartbeats           \    |
    |      |         no-idea                 \    |
    |      |         no-jpake                \    |
    |      |         no-krb5                 \    |
    |      |         no-libunbound           \    |
    |      |         no-md2                  \    |
    |      |         no-md4                  \    |
    |      |         no-mdc2                 \    |
    |      |         no-psk                  \    |
    |      |         no-rc2                  \    |
    |      |         no-rc4                  \    |
    |      |         no-rc5                  \    |
    |      |         no-sctp                 \    |
    |      |         no-sha0                 \    |
    |      |         no-srp                  \    |
    |      |         no-srtp                 \    |
    |      |         no-ssl2                 \    |
    |      |         no-ssl3                 \    |
    |      |         no-ssl3-method          \    |
    |      |         no-unit-test                 |
    |      |                                      |
    |      |     make depend                      |
    |      |     make                             |
    |      |     make install -j8                 |
    |      |                                      |
    |      |     cd ..                            |
    |      |     rm -rf $PKG                      |
    |      |     }                                |
    |      |                                      |
    |      |                                      |
    |      |                                      |
    |      |                                      |
    |      |     PKG=openssl-1.0.2a               |
    |      |                                      |
    |      |     (ARCH="linux-x86_64  enable-ec_n |
    |      | istp_64_gcc_128"                     |
    |      |     PREFIX=$(pwd)/${PKG}_64_build    |
    |      |     build                            |
    |      |     )                                |
    |      |                                      |
    |      |     (ARCH=linux-elf                  |
    |      |     PREFIX=$(pwd)/${PKG}_32_build    |
    |      |     #need: sudo yum install libstdc+ |
    |      | +-devel.i686                         |
    |      |     export CC="gcc -m32"             |
    |      |     #build                           |
    |      |     )                                |
    |      |                                      |
    |      |     wait                             |
    |      |     rm -rf $PKG                      |
    +------+--------------------------------------+





Posted by byronhe 2015-05-08
