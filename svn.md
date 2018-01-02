#### 检测svn版本
```js
    //检测svn是否安装成功
    svn --version
```

## 配置服务端
----------
#### 创建版本库
```js
    //创建版本库
    svnadmin create D:/xx/xx //版本库根目录
```
conf 配置
db 数据库
hocks 收发器
locks 锁定

#### 运行
----------
```js
    // 运行
    svnserve -d -r D:/xx/xx
    // -d 后台执行
    // -r 版本库根目录
```


----------
#### 检测服务是否启动
```js
    //检测服务是否启动
    netstat -an
    //是否有3690端口
```


----------
#### 注册window服务
```js
    sc create SVNService  binpath= "G:\subversion\bin\svnserve.exe --service -r G:\DevRep" start= auto depend= Tcpip
   /*
        sc --service control
        create 后边写创建服务的名字
        binpath 指定启动的exe文件
        start 启动方式 auto 自动启动
        depend 依赖 Tcpip
    */
```

## 客户端常用命令
#### 检出svn
```js
    // 检出svn
    svn checkout svn://localhost/OA
```
#### 添加到svn
```js
    // 添加到svn
    svn add file.xx
```
##### 关于提交权限
    x:/服务器目录/svn项目文件夹/conf/svnserve.conf
        echo svnserve.conf
        anon-access = write 设置anon权限 read|write
        # auth-access = write 设置auth权限

#### 提交
```js
    // 提交
    svn commit file.xx -m "提交说明"

    **\wangyan\MyOA\OA>svn commit README.txt -m "first svn commit about README.txt"
    增加           README.txt
    传输文件数据.
    提交后的版本为 1。
```
#### 更新
```js
    //更新本地
    svn update README.txt
    //不写 README.txt 更新全部文件
```


----------


## 关于SVN权限控制
### server端


----------


#### 打开权限设置

        echo 某SVN项目目录/conf/svnserve.conf
        // 注释掉匿名的
         anon-access = write
        // 打开授权访问
        auth-access = write

        //设置存储用户名密码信息的文件 默认是当前目录下的passwd文件
        password db = passwd

        //表示授权信息的文件 默认是当前目录下的authz
        author-db = author



----------

   #### 创建用户和密码
   ##### passwd文件

        wangyan = *****
        yixaing = ******
        testerA = *****
        testerB = ****
        boss    = ***



----------

   #### 设置权限
   ##### authz文件

        //设置组 前端开发组 wangyan，yixiang--读写权限
        // 测试组 testerA，testerB--只读权限
        [groups]
        FEteam = wangyan,yixiang
        TesterTeam = testerA,testerB

        //设置权限
        [/foo/bar] //设置权限目录位置，根目录[/]
        @FEteam = rw //read&write @代表设置组
        boss = r //boss 代表设置个人的权限
        @TesterTeam = r
        * = //表示其他人无权限



----------


## 时光机


----------


    @备注：回溯版本时，server端 某SVN项目目录/conf/svnserve.conf内的匿名设置：anon-access = none,否则报错！


----------


