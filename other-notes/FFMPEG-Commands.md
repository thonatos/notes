some ffmpeg commands .

## Base Usages

常用的一些命令。

#### Get Video

```
ffmpeg -i ./vpano_a.flv -s 1024x512 -r 30 -b:v 850k -c:a copy -c:v libx264 -profile:v baseline -an v.mp4
```

#### Get Audio

```
ffmpeg -i ./vpano_a.flv -vn -ar 44100 -ac 2 -ab 128k -f mp3 Sample.mp3
```

#### Get Poster

```
 ffmpeg -i ./v.mp4 -ss 0 -f image2 -vframes 1 poster.jpg
 # -ss 0 表示第一帧
```

#### Send rtsp

```
ffmpeg -re -i input.mp4 -c copy -f flv rtmp://192.168.3.8/hls/test
ffplay http://192.168.1.122:8080/hls/test.m3u8
```

#### convert mp4 to m3u8

```
ffmpeg -i v2048x1024.mp4 -c:v libx264 -c:a aac -strict -2 -hls_list_size 0 -hls_time 2 -f hls output.m3u8
```

## other

一些其他奇葩的应用。

```
# 直接转换
ffmpeg -i video.avi -s 320x540 -r 10 out.gif

# 另一种方法
ffmpeg -i video.avi -t 1 out%01d.gif
gifsicle --delay=10 --loop out*.gif > anim.gif
```

## More 

组合命令，可以更方便的处理一些视频。

#### convert mp4 to m3u8 (different size)

```
ffmpeg -i v2048x1024.mp4 -c:v libx264 -c:a aac -strict -2 -hls_list_size 0 -hls_time 2 -s 1920x1080 -r 30 -b:v 850k -f hls ./1920x1080/m3u8/demo.m3u8

ffmpeg -i scene2.mov -c:v libx264 -c:a aac -strict -2 -hls_list_size 0 -hls_time 2 -s 1280x720 -r 30 -b:v 3M -movflags faststart scene2@1280x720.mp4

-movflags faststart 
    ## 可在视频在未加载完之前播放

-hls_time n
## 设置每片的长度，默认值为2。单位为秒

-hls_list_size n
## 设置播放列表保存的最多条目，设置为0会保存有所片信息，默认值为5

-hls_wrap n
## 设置多少片之后开始覆盖，如果设置为0则不会覆盖，默认值为0.这个选项能够避免在磁盘上存储过多的片，而且能够限制写入磁盘的最多的片的数量

-hls_start_number n
## 设置播放列表中sequence number的值为number，默认值为0
    注意：播放列表的sequence number 对每个segment来说都必须是唯一的，而且它不能和片的文件名（当使用wrap选项时，文件名有可能会重复使用）混淆

ffmpeg -i H5小星球动态-有logo.mov -an -vcodec libx264 -pix_fmt yuv420p H5.mp4
# Mac转换的视频转码异常的处理。
 ```