## cmd到当前目录执行

```kotlin
       1、第一步：npm init

       2、第二步：npm install --production

       3、第三步： npm i vant-weapp -S --production
```

### 开发工具详细打勾npm构造

### 开发工具 工具栏点击npm构造

### 会生成一个miniprogram_npm文件夹

####  *如果报错

​	有可能是安装的时候失败了，手动删除miniprogram_npm文件夹

​	然后在官网下载vant的压缩包

​	在miniprogram_npm文件夹下把vant-weapp下面文件清空

​	把压缩包的文件夹dist复制到vant-weapp下面



引用方法

```
"usingComponents": {
        "van-button": "vant-weapp/dist/button" 
}
```

​	