# -flto 编译报错修复

## Windows

将 `[MinGW-w64安装路径]\libexec\gcc\x86_64-w64-mingw32\[版本号]\`  下的 `liblto_plugin.dll` 和 `liblto_plugin.dll.a` 复制到 `[MinGW-w64安装路径]\lib\bfd-plugins`

## Linux

```bash
sudo ln -s /usr/lib/gcc/x86_64-linux-gnu/[gcc版本号]/liblto_plugin.so /usr/lib/bfd-plugins/liblto_plugin.so

```
