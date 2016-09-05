# android-adb-ffmpeg
利用adb录制android手机屏幕操作，然后把录制好的mp4转化成为gif图（主要利用ffmpeg）
# adb screenrecord命令录制mp4
```shell
adb shell screenrecord /sdcard/demo.mp4
```
# adb pull导出mp4到本地
```shell
adb pull /sdcard/demo.mp4 ./Downloads/
````
# ffmpeg把mp4转化为高质量gif图
直接使用ffmpeg转化出来的gif图，效果不好，网上找了一圈，主要的思路是向利用视频文件生存调色板，然后再转化gif图，详情看[这里](http://blog.pkh.me/p/21-high-quality-gif-with-ffmpeg.html)。
使用如下脚本，即gifenc.sh：
```shell
#!/bin/sh
palette="/tmp/palette.png"
filters="fps=15,scale=320:-1:flags=lanczos"

ffmpeg -v warning -i $1 -vf "$filters,palettegen" -y $palette
ffmpeg -v warning -i $1 -i $palette -lavfi "$filters [x]; [x][1:v] paletteuse" -y $2
```
注意需要该脚本依赖[ffmpeg](https://ffmpeg.org/)
## 运行脚本转化gif
```shell
./gifenc.sh video.mkv anim.gif
```
