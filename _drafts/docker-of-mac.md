docker 命令

- 容器生命周期管理 — docker [run|start|stop|restart|kill|rm|pause|unpause]
- 容器操作运维 — docker [ps|inspect|top|attach|events|logs|wait|export|port]
- 容器rootfs命令 — docker [commit|cp|diff]
- 镜像仓库 — docker [login|pull|push|search]
- 本地镜像管理 — docker [images|rmi|tag|build|history|save|import]
- 其他命令 — docker [info|version]


attach
build
commit
cp
- create
- deploy
diff
events
- exec
export
history
images 列出机器上的镜像
import
info
inspect
kill
- load
login
- logout
logs
- network
- node
pause
- plugin
port
ps
pull
push
- rename
restart
rm
rmi
run
save
search
- service
- stack
start
- stats
stop
- swarm
tag
top
unpause
- update
version
- volume
wait

docker run -itd -p 2023:22 -p 80:8080 -p 8080:8080 -v /Users/scsidisk/repo/nginx-vhots2/:/home/work/app/nginx/conf/vhosts/ --dns=10.237.39.2   -v /Users/scsidisk/repo/xiaomi2:/home/work/data/www/ --name xmwebsite 10.237.36.179:5000/develop:centos6.6  /usr/bin/supervisord

