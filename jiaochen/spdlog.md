# spdlog上手教程

↩️[返回主页]

Github源码[spdlog-github]

注： __不建议使用安装package的方式，版本似乎比较旧，不能参照官方readme使用__

## 1.使用源码编译后install的方式

```sh
git clone https://github.com/gabime/spdlog.git
cd spdlog && mkdir build && cd build
cmake .. && make -j
```

最新版本CMake需要版本3.11

查看当前cmake版本

```sh
cmake --version
```

如果编译时发生错误，建议先将git切换到v1.11.0的tag分支

```sh
git checkout v1.11.0
```

然后再执行cmake和make

然后可以install到系统库中

```sh
make install
```

如果显示权限不足则添加sudo前缀

```sh
sudo make install
```

## 2.安装后即可在自己的cpp中使用

### i.引入头文件

```sh
#include "spdlog/spdlog.h"
#include "spdlog/sinks/rotating_file_sink.h"
```

### ii.配置log对象并指定文件数量和单个文件的大小上限

```sh
auto max_size = 1048576 * 5;
auto max_files = 3;
auto fileDir = "/home/slamopto/log_test/myNodeLog.txt";
auto logger = spdlog::rotating_logger_mt("logger", fileDir, max_size, max_files);
```

### iii.使用log对象输出日志

```sh
logger->info("This is an info msg");
logger->error("This is an err msg");
```

_注：_ spdlog存储在缓冲区，需要达到4kb才会写入文件一次（或者关闭软件会直接写入。不能直接断电，直接断电似乎会丢失缓存区的日志）。如果想要在某些特定的logger->info或其他写入后即刻刷新，需要添加如下语句手动进行刷新

```sh
logger->flush();
```

### iv.查看指定路径下的日志文件保存情况

### v.其他常用日志格式

```sh
// Log an informational message
spdlog::info("Welcome to spdlog!");
spdlog::error("Some error message with arg: {}", 1);

spdlog::warn("Easy padding in numbers like {:08d}", 12);
spdlog::critical("Support for int: {0:d};  hex: {0:x};  oct: {0:o}; bin: {0:b}", 42);
spdlog::info("Support for floats {:03.2f}", 1.23456);
spdlog::info("Positional args are {1} {0}..", "too", "supported");
spdlog::info("{:<30}", "left aligned");
```

## 3.更多高阶玩法请参照官方Github的README

[返回主页]:../README.md
[spdlog-github]: https://github.com/gabime/spdlog