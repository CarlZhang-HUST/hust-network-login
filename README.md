# HUST-Network-Login

极简主义的华中科技大学校园网络认证工具，支持有线和无线网络。下载即用，大小约为 400k，静态链接无依赖。为路由器等嵌入式设备开发，支持所有主流硬件软件平台。No Python, No Dependencies, No Bullshit.

## 使用

从 Release 下载对应硬件和操作系统平台的可执行文件。

配置文件只有两行, 第一行为用户名，第二行为密码，例如

```text
M2020123123
mypasswordmypassword
```

保存为 my.conf

然后运行

```shell
./hust-network-login ./my.conf
```

my.conf 是刚才的配置文件，你可以换成其他名字。

连接成功后，程序将会每间隔 15s 向上级路由器发送ipv6心跳包以保持活跃状态，并ping baidu.com 测试一次网络连通性。如果无法连接则进行重新登陆。

---

除上述参数化 *附加配置文件路径* `./my.conf` 的方式，另支持以下2种方法，以实现无参数运行

1. 通过环境变量 `HUST_NETWORK_LOGIN_USERNAME` 与 `HUST_NETWORK_LOGIN_PASSWORD` 来分别配置用户名，密码

2. 将上述 `my.conf` 保存为默认配置文件 `/etc/hust-network-login.conf` 或 `/etc/hust-network-login/config`

运行

```shell
./hust-network-login
```

即可
 
## 编译

编译本地平台只需要使用 `cargo`。

```shell
cargo build --release
strip ./target/release/hust-network-login
```

strip 可以去除调试符号表，将体积减少到 500k 以下。

交叉编译推荐使用 `cross`，当然你也可以自己手动配置工具链。

```shell
cargo install cross
cross build --release --target mips-unknown-linux-musl
mips-linux-gnu-strip ./target/mips-unknown-linux-musl/release/hust-network-login
```

你应当根据自己的路由器平台选择硬件平台。支持的目标平台戳[这里](https://github.com/rust-embedded/cross)。
