# Linux 下编译 OpenCV + Contrib

- 此处使用官方的国内镜像，以解决各种无法连接外网导致的下载问题。
  - OpenCV
    - https://gitcode.net/opencv/opencv.git
  - OpenCV_Contrib
    - https://gitcode.net/opencv/opencv_contrib.git

- 克隆OpenCV与OpenCV_Contrib的源代码。以下使用 `[OpenCV_DIR]` 来代指OpenCV的克隆目录。

   此处使用 `-b` 指定版本标签。

   `OpenCV_Contrib` 需克隆至 `[OpenCV_DIR]/contrib/` 目录内。
- 在 `[OpenCV_DIR]` 下新建目录 `build` 。
- 在 `[OpenCV_DIR]/build` 下执行指令以生成 `Makefiles`。

```bash
cmake \
-D OPENCV_ENABLE_NONFREE=ON \
-D OPENCV_EXTRA_MODULES_PATH=~/program/_deps/opencv-470-x64/contrib/modules \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D CMAKE_INSTALL_PREFIX=/usr/local/opencv \
-D WITH_INF_ENGINE=ON \
-D WITH_TBB=ON \
-D WITH_FFMPEG=ON \
-D WITH_OPENMP=ON \
-D WITH_IPP=ON \
..
```

- 在 `[OpenCV_DIR]/build` 目录下执行 `make -j12` ，并等待一段时间(大约30min)，直到编译工作全部完成。

   编译指令中的 `12` 为使用的并行CPU核心数，可以自行修改以提高编译效率。
- 在 `[OpenCV_DIR]/build` 目录下执行 `sudo make install` ，并等待安装工作全部完成。
- 新建文件 `/etc/ld.so.conf.d/opencv.conf` 并在其中添加以下内容：

```
/usr/local/opencv/lib
```

- 执行 `sudo ldconfig` 以刷新配置文件。
- 在 `/etc/bash.bashrc` 中添加以下内容：

```
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/opencv/lib/pkgconfig
export PATH=$PATH:/usr/local/opencv/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/opencv/lib
```

- 执行 `source /etc/bash.bashrc` 以刷新配置文件。
- 此时OpenCV的编译与配置工作已全部完成！
