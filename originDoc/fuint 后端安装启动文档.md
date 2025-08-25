fuint 后端安装启动文档 
运行软件版本要求：
1）java1.8 （必须 jdk1.8）
2）mysql5.7 或 mysql5.8 
3）redis 版本无限制，推荐 redis3.2
本地 idea 启动 
导入 fuintBackend 后端工程。fuint-framework 是项目框架，fuint-application 是业务逻
辑代码，是打包工程。fuint-application 依赖 fuint-framwork、fuint-utils、fuint￾repository 模块。
1）idea 插件的安装： 
idea 需要先安装 Lombok 插件，如下图
第 1 步，选择 File->Settings
第 2 步，选择 Plugins 并搜索 Lombok
点击 install，并重启 idea 即可。
2）修改环境变量和配置文件路径，如下图所示： 
3）找到入口函数，如下图所示： 
 
linux 下部署步骤 
在右侧的 maven 的 fuint（root）下执行 install，切记要先修改环境变量再打包，否则会
找不到配置文件。如下图：
1. 将打包生成的 fuint-application-1.0.0.jar 包上传到服务器自定义路径，如目录
/usr/local/fuint。
2. 上传存放环境配置参数信息的 configure 目 录 到 服 务 器 指 定 目 录 下 ， 如 ：
/usr/local/fuint/configure/下
（1）修改 configure/目录下当前环境下 application.properties 文件中数据库配置信息
等：
（2）修改环境变量，如下图所示
文件路径：fuintBackend\fuint￾application\src\main\resources\application.properties
env.profile=dev 表示开发环境，env. profile=prod 表示生产环境。
env.properties.path 是配置文件的绝对路径，指向 fuintBackend 下的 configure 目
录。linux 下可配置/www/wwwroot/configure。windows 下可配置：C:\Code\fuint\configure。
3. 执行启动命令：
nohup java -Dfile.encoding=UTF-8 -Xmx2048m -Xms2048m -Xss256k -Xmn1024m -jar 
fuint-application-1.0.0.jar > /dev/null 2>&1 &
4. 默认日志路径/data/log/fuint，需要 755 写入权限；
5. Nginx 设置伪静态，将请求转发给后端，从而解决跨域问题：
location /fuint-application/ {
proxy_pass http://localhost:后端启动端口/;
client_max_body_size 5m;
proxy_set_header host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
 
注意事项 
如果修改 fuint-repository、fuint-framework、fuint-utils 框架的话，修改完成后需要
重新 install 到 maven 私服即可。
需要注意的是 fuint-repository、fuint-framework、fuint-utils 中的版本号不要轻易修
改，以免影响 fuint-application 服务应用。
 
configure 文件介绍 
这里所说的 config 文件是指后台 web 系统的配置文件，目前 config 目录下有三个文件夹，
分别是 dev，test，prod，分别代表开发环境、测试环境以及生产环境的配置文件。
宝塔部署步骤
1、安装宝塔，使用腾讯云的话，在系统安装时选择宝塔系统。
2、安装 jdk，在应用商店搜索“java”，选择软件 Java 项目一键部署 3.4。
3、安装 mysql，在应用商店搜索“mysql”并安装，然后导入数据。
4、网站->添加 java 项目，选择“spring_boot”，选择 jar 包目录。
5、选择前后端分离，将前端打包，上传到设置的前端目录。（也可以单独新增
一个后台前端的网站）
6、上传配置文件，必须上传到和 jar 包中设置的路径一致，比如
/www/wwwroot/configure。
7、创建默认日志路径/data/log/fuint/，赋予写入 755 权限。
8、重启网站服务。
9、设置伪静态，将请求转发给后端，从而解决跨域问题：
location /fuint-application/ {
proxy_pass http://localhost:后端启动端口/;
client_max_body_size 5m;
proxy_set_header host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}