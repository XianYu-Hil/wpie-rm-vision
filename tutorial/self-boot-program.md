# Ubuntu 开机自启


## 程序


- 在 /usr/share/applications 下创建一个 `.desktop` 文件，文件名是需要开机自启的可执行文件。后缀为 `(application/x-sharedlib)`。

```bash
cd /usr/share/applications

sudo gedit Visual.desktop  #创建一个桌面文件，其中Visual为可执行文件名
```

- 在其中输入：

```
[Desktop Entry]
Version=1.0
Name=Visual
Exec=cd /path/to/application/directory/ ; ./application-to-boot
Terminal=true
StartupNotify=true
NoDisplay=true
Type=Application
Categories=System;Utility;Archiving;
```

    `/path/to/application/directory/` 请修改为可执行文件所在目录。`./application-to-boot` 请修改为可执行文件名。

- 保存并将该文件拷贝到/etc/xdg/autostart目录下

```bash
cp /usr/share/applications/Visual.desktop /etc/xdg/autostart
```

## 看门狗

- 创建一个 shell 文件

```bash
sudo gedit watch_dog.sh
```

- 在其中输入：

```bash
#!/bin/sh
check=`ps -ef| grep /path/to/application|grep -v grep |awk '{print $2}'`
while true;
do
if [ ! $check ] 
then
/path/to/application
fi 
done
```
    `/path/to/application` 请修改为可执行文件路径

- 请先进行测试以确保看门狗脚本无误

```bash
sh watch_dog.sh
```

- 当看门狗脚本测试无误后，将其与其它开机自启的程序一样，将其加入开机自启中。其中 `.desktop` 文件内容需修改为：

```bash
[Desktop Entry]
Type = Application
Exec=gnome-terminal -x bash -c "/path/to/watch_dog.sh" # 看门狗shell脚本的路径
Hidden=false
Terminal=true
StartupNotify=true
NoDisplay=true
X-GNOME-Autostart-enabled=true
Name=watch-dog
Comment=Watch Dog
```

    `/path/to/watch_dog.sh` 请修改为看门狗脚本路径。

为了安全起见，**开启自启不能与看门狗启动同一个程序！**比如不能看门狗启动Visual，开机自启也启动Visual。