## 图片加载笔记

> 会对[Glide](https://github.com/bumptech/glide) Picasso Fresco及Sketch进行剖析，选出最适合快速开发的图片加载框架

#### 对比
| 对比项 | Glide | Picasso | Fresco | Sketch |
|:---:|:---:|:---:|:---:|:---:|
| 作者 | bumptech |  |  |  |
| 公司 | google推荐 |  |  |  |
| 时间 | 2014 |  |  |  |
| 版本 | 4.4.0 | 2.5.2 |  | 2.6.0 |
| Bitmap格式 | RGB_565 | ARGB_8888 |  |  |
| 加载速度 | 1 | 2 |  |  |
| 支持Gif | 支持 | 不支持 |  |  |
| 播放本地视频 | 支持 | 不支持 |  |  |
| thumbnail | 支持 | 不支持 |  |  |
| [库的大小](https://github.com/bumptech/glide/releases/tag/v4.4.0) | 637.2KB |  |  |  |
| 方法数 |  | [849](http://www.methodscount.com/?lib=com.squareup.picasso%3Apicasso%3A2.5.2) |  | 4224 ([3512](http://www.methodscount.com/?lib=me.panpf%3Asketch%3A2.6.0)+[712](http://www.methodscount.com/?lib=me.panpf%3Asketch-gif%3A2.6.0)) |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |

#### Glide使用笔记

> 阅 [文-Glide介绍](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0327/2650.html)、[郭霖-Glide最全解析](http://blog.csdn.net/column/details/15318.html)、[官方文档]()得出的笔记

- Glide和Activity/Fragment的生命周期绑定，so. Glide.with(#) #尽量直接传Activity或Fragment，不传Context
- Glide提供了自定义模块的功能，建议更改Glide的Bitmap格式和缓存地址，[案例](https://github.com/zwping/PNotes/blob/master/AndroidNotes/imageloader/src/main/java/win/zwping/imageloader/glide/GlideModule.java)，<b>√建议使用</b>
- Glide加载图片的大小和ImageView大小一致，默认会对不同大小ImageView Glide都会进行不同大小图片的缓存，所以Glide占用内存更多
- Glide默认只缓存加载大小的图片，每次重新加载至不同大小的ImageView时都会重新拉取Url，在这建议Glide设置同时缓存全尺寸和不同大小的尺寸[案例]()，下次再加载至不同大小的ImageView，会从缓存中取全尺寸的图片，调整大小缓存并显示，<b>√建议使用</b>
- Glide方法数过于庞大，建议开启[ProGuard](https://github.com/bumptech/glide#proguard)，<b>√建议使用</b>
- Glide加载gif时，一定要用`diskCacheStrategy(DiskCacheStrategy.SOURCE)`或`NONE`，不然会将gif每一帧都压缩缓存，超级耗时
- Glide预加载preload()时，一定要用diskCacheStrategy(DiskCacheStrategy.SOURCE)缓存全尺寸图片
- Glide默认有图片转换功能，如果需要加载原始图片大小，建议使用`override(Target.SIZE_ORIGINAL, Target.SIZE_ORIGINAL)`，不建议直接取消Glide图片转换功能`.dontTransform()`
- 在一个页面中加载大量图片，建议不使用过渡动画，[说明](https://muyangmin.github.io/glide-docs-cn/doc/transitions.html#性能提示)

> Glide FQA

- <font color="#F08080">Glide怎么监听Activity/Fragment的生命周期</font>
<br />
添加隐藏的Fragment，监听其生命周期达到with()传入值对应的生命周期 [示例](https://github.com/bumptech/glide/blob/master/library/src/main/java/com/bumptech/glide/manager/SupportRequestManagerFragment.java)
<br />