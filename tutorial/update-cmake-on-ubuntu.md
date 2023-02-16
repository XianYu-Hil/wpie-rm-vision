# Ubuntu 更新 CMake

- 前往 `https://cmake.org/download/` 下载所需版本的源文件，并解压。
- 执行配置文件

```bash
sudo ./configure
```

- 执行编译并等待

```bash
sudo make -j12
```

- 执行安装并等待

```bash
sudo make install
```

- 软件版本管理注册并设置为默认版本

```bash
sudo update-alternatives --install /usr/bin/cmake cmake /usr/local/bin/cmake 1 --force
```
