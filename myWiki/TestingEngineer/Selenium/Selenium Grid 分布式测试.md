# Grid

## download and install Grid
官网下载即可


## Standalone(单机版本)

## Hub and Node

使用 jar 包，启动服务

start with hub

```shell
java -jar selenium-server-standalone.jar -role hub -port
```

Start the Nodes

```shell
java -jar selenium-server-standalone.jar -role node -hub http://localhost:4444
```

推荐使用配置文件进行配置
java -jar selenium-server-standalone.jar -role hub -hubConfig HubConfig -port -debug -log log.txt
## Distributed

## Docker

16:14:01.517 INFO [Hub.start] - Selenium Grid hub is up and running
16:14:01.519 INFO [Hub.start] - Nodes should register to http://172.20.10.4:8002/grid/register/
16:14:01.519 INFO [Hub.start] - Clients should connect to http://172.20.10.4:8002/wd/hub

文档地址：https://www.selenium.dev/documentation/en/grid/grid_3/components_of_a_grid/

开启 log
java -jar selenium-server-standalone.jar -role hub -log log.txt

开启 debug 模式
java -jar selenium-server-standalone.jar -role hub -debug

苹果浏览器是不需要下载驱动的，在设置里面打开即可；
