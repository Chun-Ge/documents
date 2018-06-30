# 部署方法说明

## Frontend
前端使用vue+pug+typescript作为基础技术栈，部署，需要npm版本至少3以上，node版本至少8以上的环境

部署Frontend：

- npm install 下载需要的模块
- npm build，利用webpack自动化打包，编译，生成最终文件

注意：
- npm 下载包时候，可能遇到慢速，甚至无法下载的错误，可以选用淘宝镜像源的cnpm

## Backend

后台使用Go开发，框架采用[iris](https://github.com/kataras/iris)，使用[go-xorm](https://github.com/go-xorm/xorm)连接mysql数据库，另外使用[cron](https://github.com/robfig/cron)处理log文件，[cobra](https://github.com/spf13/cobra)和[viper](https://github.com/spf13/viper)处理命令行参数。

部署Backend服务，需要先启动mysql，并且创建数据库`test_shudong`。 下面以mysql的`root`用户为例。

```bash
$ mysql -u root -p
# (enter password)
# ...
mysql> create database test_shudong;
# (Output:) Query OK, 1 row affected (0.00 sec)
```

然后用`go get`将代码仓库拉取到本地， 修改`config/.cobra.yaml`中对应的选项，然后运行。

选项具体包括：

- `PORT`: 后台服务运行时绑定的端口，默认为8000。
- `MySqlURL`: 运行mysql服务的服务器地址，默认为`localhost`。
- `MySqlPort`: mysql端口号，默认为`3306`。
- `MySqlUser`: 登陆mysql的用户名，默认为`root`。
- `MySQLPassword`: 登陆mysql的密码，默认为`root`。
- `ProductionMode`: Production模式。默认为false，即DEBUG模式。
  - Production模式时，日志文件会生成在`/var/log/shudong-sysu/`下，所以使用此模式时大多数情况需要`sudo`权限。
  - DEBUG模式时，日志文件会生成在`./log`下。
  - 两种方式生成的日志文件均以日期划分文件。

配置文件中的配置也可以通过运行时以`-`方式进行参数指定，使用该方法指定的参数将会覆盖`.cobra.yaml`中的设置。具体参见`go run main.go --help`。

```sh
go get -u -v github.com/Chun-Ge/Shudong-Backend
cd $GOPATH/src/github.com/Chun-Ge/Shudong-Backend
go build && ./Shudong-Backend # 或 go run main.go
```

注意：

- `.cobra.yaml`是运行时读取，所以即使采用build方式(而非`go run main.go`)，也可以在build之后再修改配置文件。
- `go get`安装`golang.org`下面的数据包可能会遇到网络问题，可以参考[这篇文章](https://studygolang.com/articles/10263)。
