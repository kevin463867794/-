#HBuilder
@(H5开发工具)[DCloud工具|webapp|by 陈桂元]

##打包方式(两种)
#### 1.[app云端打包](http://ask.dcloud.net.cn/docs/#http://ask.dcloud.net.cn/article/111)
####2.离线打包方式:
##### (1)android离线打包方式
 - 首先[下载HBuilder离线打包Android版SDK](http://ask.dcloud.net.cn/article/103)
 - 把项目的html+css+js方法sdk的项目中。
 - build出```apk```文件
 - 打包详情[详情](http://ask.dcloud.net.cn/article/38)

##### (2)ios离线打包方式
 - 首先[下载HBuilder离线打包ios版SDK](http://ask.dcloud.net.cn/article/103)
 - 把项目的html+css+js方法sdk的项目中。
 - build出```ipa```文件
 - 打包详情[详情](http://ask.dcloud.net.cn/article/41)

##### (3)前面说的是app的打包，至于web的,他本来就是html+css+js。所以就直接放上去就行。
##升级问题

####1.[整包(apk/ipa)升级](http://ask.dcloud.net.cn/article/431)
- 适用于大版本更新，runtime发生变化时（模块、配置、版本等变化）必须使用此更新方法

####2.[资源升级(wgt)](http://ask.dcloud.net.cn/article/182) 
- 适用于小版本更新 。runtime不变，前端页面整体压缩包更新

####3.[应用资源差量升级](http://ask.dcloud.net.cn/article/199)
- 适用于小版本更新 。runtime不变，前端页面仅需要更新的部分更新。
