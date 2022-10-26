# video

> https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video
>
> [使用 JS 获取视频 Codec (jackpu.com)](https://jackpu.com/shi-yong-js-huo-qu-shi-pin-codec/)
>
> 素材[Download Sample Videos / Dummy Videos For Demo Use (sample-videos.com)](https://sample-videos.com/index.php#sample-mp4-video)

## 使用

```html
<video width="320" height="240" controls>
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.ogg" type="video/ogg">
    您的浏览器不支持 video 标签。
</video>
```

## 自带属性&方法

| 属性 / 方法      | desc                                  |
| ---------------- | ------------------------------------- |
| currentTime      | 当前视频播放时间(s)                   |
| duration         | 当前视频播放时长(s)                   |
| volume           | 声音[0-1]                             |
| paused           | 暂停                                  |
| ended            | 结束                                  |
| play()           | 播放                                  |
| pause()          | 暂停                                  |
| loadedmetadata() | 视频加载获取数据(获取duration)        |
| timeupdate()     | 视频变化事件, (获取实时的currentTime) |
| ended()          | 视频播放结束事件                      |
| volumechange()   | 视频声音事件                          |

