常用命令


查看端口是否打开
使用 netstat 命令
a. `netstat -nat | grep <端口号>`  , 如命令 `netstat -nat | grep 3306`
b. `netstat -nat |grep LISTEN`


1. 图片缩放 image magic

for img in `ls *.jpg`; do convert -resize "1280x1280>" $img small/$img; done

2. 图片剪切 Gif gifsicle

gifsicle --crop 0,0-200,200 --output out.gif copper-loader.gif

3. 文件夹大小

IFS=$'\n';for x in $(ls); do echo `sudo du -sh "$x"`; done
for x in `ls -1`; do echo `sudo du -sm "$x"`; done
ls -1| while read line;  do echo `sudo du -sm "$line"`; done

4. RMVB to MP4

brew install ffmpeg
ffmpeg -i input.rmvb -c:v libx264 -preset veryfast -crf 18 -c:a copy output.mp4

5. 日志 ip 分析

find ./ -name *log* -exec cat {} \;| awk '{arr[$1]++;}END{for(i in arr){print i , arr[i] }}'  | sort -n -k 2

time  awk '{arr[$1]++;}END{for(i in arr){print i , arr[i] }}' test.txt | sort -n -k 2   

mkdir wechat-log-2016-05013
mv nginx.log.20160513.tgz wechat-log-2016-05013
cd wechat-log-2016-05013
find ./ -name *.tgz -exec tar xzvf  {} \;

6. 微信预约用户数量

$ grep -E '13/May/2016:06:00|13/May/2016:06:01|13/May/2016:06:02|13/May/2016:06:03|13/May/2016:06:04' act.men.mi.com.log.20160513|awk '{print $7}'|grep 'token?code='|sort |uniq -c |wc -l
   13229

7. 微信预约用户数量

cat /home/work/logs/nginx/act.men.mi.com.log |grep '22/Apr/2016'|grep '/token?code='|wc -l

8. 视频转码

Linux系统，相机和手机拍摄的视频，MP4或者其他格式，用mencoder，h264，mp3重新转码压缩，图像质量不变文件缩小一半

`ls -l --time-style=full-iso $i|awk 'gsub(/:/,"-"){print $6"_"substr($7,1,8)"_"substr($9,4,5)".avi"}'` 是根据文件日期设置文件名，包括日期时间，前提条件是文件创建的日期时间要正确。 
#!bin/bash for i in *.{MP4,MOV,AVI}; do     mencoder -oac pcm -vf harddup -ovc x264 -ffourcc H264 $i -o `ls -l --time-style=full-iso $i|awk 'gsub(/:/,"-"){print $6"_"substr($7,1,8)"_"substr($9,4,5)".avi"}'` ;    mv $i ~/.local/share/Trash/files/; done 
Mac系统，现在的华为手机拍摄的mp4视频本身名字就带有日期 时间，所以名字就不再改了。Mac上用VLC更方便，mencoder似乎没有可以直接可以用的执行文件，只有播放器。

#!/bin/bash  for a in *.{mp4,MP4}; do  /Applications/VLC.app/Contents/MacOS/VLC -I dummy "$a" --sout "#transcode{vcodec=h264,vb=3072,acodec=mp3,ab=128}:standard{mux=avi,dst=`expr ${a:0:19}.avi`,access=file}" vlc://quit  done

9. wget 下载整个网站，或者特定目录

wget -c -r -np -k -L -p www.xxx.org/pub/path/

10. ssh 修改terminal 标题问题修复

~/.bash_profile 中添加

alias ssh='TERM=vt100 /usr/bin/ssh'

11 别人下载的软件，改成自己下载的

加载dmg文件后将NN.app提取到桌面，然后拷贝你自己在MAS下载的任意免费App内的“_CodeSignature”、“_MASReceipt”，“CodeResources”这三个文件，替换NN.app中相应的同名文件即可。

12 重建/usr/local/Cellar/ctags/5.8/bin/ctags -R -f .tags

13 brew清理包和 img

brew update && brew upgrade && brew cleanup

14 Disable the Dashbaord

Thankfully, it’s really easy to disable the Dashboard — open up a Terminal and paste this in:
defaults write com.apple.dashboard mcx-disabled -boolean YES && killall Dock
To reverse it and re-enable it, use this:
defaults write com.apple.dashboard mcx-disabled -boolean NO && killall Dock


