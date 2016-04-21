Fail when go get gopkg.in
=========================

David February 17, 2016

One may comes across some error on install yaml.v2 using `go get`, like:

    $ go get gopkg.in/yaml.v2
    package gopkg.in/yaml.v2: unrecognized import path "gopkg.in/yaml.v2"

Usually it’s a network error, please try:

    $ go get -v gopkg.in/yaml.v2
    Fetching https://gopkg.in/yaml.v2?go-get=1
    https fetch failed.
    import "gopkg.in/yaml.v2": https fetch: Get https://gopkg.in/yaml.v2?go-get=1: x509: certificate signed by unknown authority
    package gopkg.in/yaml.v2: unrecognized import path "gopkg.in/yaml.v2"

If this is what you get, then just  type:


    $ go get -insecure gopkg.in/yaml.v2

And that’s all you need, enjoy :)