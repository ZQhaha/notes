[TOC]

# 阿里云盘CLI使用手册

## 帮助命令

> ```shell
> aliyunpan > help
> 
> ...
>    阿里云盘:
>      album, abm   相簿(Beta)
>      cd           切换工作目录
>      download, d  下载文件/目录
>      ls, l, ll    列出目录
>      mkdir        创建目录
>      mv           移动文件/目录
>      pwd          输出工作目录
>      recycle      回收站
>      rename       重命名文件
>      rm           删除文件/目录
>      share        分享文件/目录
>      sync         同步备份功能
>      upload, u    上传文件/目录
>      webdav       在线网盘服务
> ...
> ```

## 登录

> ```shell
> aliyunpan > login -RefreshToken=32994cd2c43...4d505fa79
> 
> 阿里云盘登录成功:  tickstep
> aliyunpan:/ tickstep$ 
> ```
>
> 或者
>
> ```shell
> bash ./aliyunpan login -RefreshToken=32994cd2c43...4d505fa79
> ```

## 上传

> ```shell
> NAME:
>    aliyunpan.exe upload - 上传文件/目录
> 
> USAGE:
>    aliyunpan upload <本地文件/目录的路径1> <文件/目录2> <文件/目录3> ... <目标目录>
> 
> OPTIONS:
>    -p value         本次操作文件上传并发数量，即可以同时并发上传多少个文件。0代表跟从配置文件设置（取值范围:1 ~ 20） (default: 0)
>    --retry value    上传失败最大重试次数 (default: 3)
>    --timeout value  上传请求超时时间，单位为秒。当遇到网络不好导致上传超时可以尝试调大该值，建议设置30秒以上 (default: 0)
>    --np             no progress 不展示上传进度条
>    --ow             overwrite, 覆盖已存在的同名文件，注意已存在的文件会被移到回收站
>    --norapid        不检测秒传。跳过费时的SHA1计算直接上传
>    --driveId value  网盘ID
>    --exn value      exclude name，指定排除的文件夹或者文件的名称，只支持正则表达式。支持同时排除多个名称，每一个名称就是一个exn参数
>    --bs value       block size，上传分片大小，单位KB。推荐值：1024 ~ 10240。当上传极大单文件时候请适当调高该值 (default: 10240)
> 
> Example:
> 	1. 将本地 200GB 极大文件 C:\Users\Administrator\Desktop\1.mp4 上传到网盘 /视频 目录，需要调高上传分片大小
>     >>>aliyunpan upload -bs 30720 C:/Users/Administrator/Desktop/1.mp4 /视频
>     2. 覆盖上传，已存在的同名文件会被移到回收站
>     >>>aliyunpan upload -ow 1.mp4 /视频
>     3. 将本地的 C:\Users\Administrator\Video 整个目录上传到网盘 /视频 目录，但是排除所有的.jpg文件和.mp3文件，每一个排除项就是一个exn参数
>     >>>aliyunpan upload -exn "\.jpg$" -exn "\.mp3$" C:/Users/Administrator/Video /视频
>     4. 将本地的 C:\Users\Administrator\Video 整个目录上传到网盘 /视频 目录，但是排除所有的 @eadir 文件夹
>     >>>aliyunpan upload -exn "^@eadir$" C:/Users/Administrator/Video /视频
> ```
>
> 

## 下载

> ```shell
> NAME:
>    aliyunpan.exe download - 下载文件/目录
> 
> USAGE:
>    aliyunpan download <文件/目录路径1> <文件/目录2> <文件/目录3> ...
>    
> OPTIONS:
>    --ow             overwrite, 覆盖已存在的文件
>    --status         输出所有线程的工作状态
>    --save           将下载的文件直接保存到当前工作目录
>    --saveto value   将下载的文件直接保存到指定的目录
>    -x               为文件加上执行权限, (windows系统无效)
>    -p value         指定同时进行下载文件的数量（取值范围:1 ~ 20） (default: 0)
>    --retry value    下载失败最大重试次数 (default: 3)
>    --nocheck        下载文件完成后不校验文件
>    --np             no progress 不展示下载进度条
>    --driveId value  网盘ID
>    --exn value      exclude name，指定排除的文件夹或者文件的名称，被排除的文件不会进行下载，只支持正则表达式。支持同时排除多个名称，每一个名称就是一个exn参数
>    
> Example:
> >>>aliyunpan download /我的资源/*.mp4
> 
> >>>aliyunpan download -exn "\.jpg$" /我的资源
>     "以下是典型的排除特定文件或者文件夹的例子，注意：参数值必须是正则表达式。在正则表达式中，^表示匹配开头，$表示匹配结尾。"
>     1)排除@eadir文件或者文件夹：-exn "^@eadir$"
>     2)排除.jpg文件：-exn "\.jpg$"
>     3)排除.号开头的文件：-exn "^\."
>     4)排除~号开头的文件：-exn "^~"
>     5)排除 myfile.txt 文件：-exn "^myfile.txt$"
>     
> >>>aliyunpan download --saveto d:/panfile /我的资源/1.mp4
> ```