# sudo 免密

1. 打开终端，并在命令框中输入 ：

```

sudo visudo

```

2. 这时会提示你输入sudo密码，输入密码之后弹出一个文件。在文件中找到 `%sudo ALL=(ALL:ALL) ALL` ，在这一行下添加如下内容：

```

wpie ALL=(ALL) NOPASSWD: ALL #wpie为用户名

```

3. 之后按**Ctrl+O**，接着按**回车**确定保存文件。最后**Ctrl+X**退出编辑。
