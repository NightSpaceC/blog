---
author: "Night Space"
title: 去除 aria2 下载连接数限制
date: 2023-04-02T11:54:00+08:00
lastmod: 2023-04-02T11:54:00+08:00
tags:
    - 工具
    - 下载
categories:
    - 工具
    - 下载
---
aria2支持多连接下载，当网站连接限速时可大幅提升下载速度

aria2修改配置可使单服务器最多16连接

但对于KB级的限速，这依然难以接受

要想打破16连接的限制，需要修改源码再自己重新编译

经过一段时间的摸索，这是补丁

```patch
--- Dockerfile.mingw
+++ Dockerfile.mingw
@@ -0,7 +0,7 @@ 
 RUN curl -L -O https://gmplib.org/download/gmp/gmp-6.2.1.tar.lz && \
     curl -L -O https://github.com/libexpat/libexpat/releases/download/R_2_4_1/expat-2.4.1.tar.bz2 && \
     curl -L -O https://www.sqlite.org/2021/sqlite-autoconf-3360000.tar.gz && \
-    curl -L -O http://zlib.net/zlib-1.2.11.tar.gz && \
+    curl -L -O http://zlib.net/fossils/zlib-1.2.11.tar.gz && \
     curl -L -O https://c-ares.haxx.se/download/c-ares-1.17.2.tar.gz && \
     curl -L -O https://www.libssh2.org/download/libssh2-1.9.0.tar.gz && \
     curl -L -O https://github.com/libssh2/libssh2/commit/ba149e804ef653cc05ed9803dfc94519ce9328f7.patch
@@ -0,6 +0,6 @@
         LIBS="-lws2_32" && \
     make install
 ADD https://api.github.com/repos/aria2/aria2/git/refs/heads/master version.json
-RUN git clone https://github.com/aria2/aria2 && \
-    cd aria2 && autoreconf -i && ./mingw-config && make && \
+COPY . /aria2/
+RUN cd aria2 && autoreconf -i && ./mingw-config && make && \
     $HOST-strip src/aria2c.exe
--- src/OptionHandlerFactory.cc
+++ src/OptionHandlerFactory.cc
@@ -0,7 +0,7 @@
   {
     OptionHandler* op(new NumberOptionHandler(PREF_MAX_CONNECTION_PER_SERVER,
                                               TEXT_MAX_CONNECTION_PER_SERVER,
-                                              "1", 1, 16, 'x'));
+                                              "1", 1, -1, 'x'));
     op->addTag(TAG_BASIC);
     op->addTag(TAG_FTP);
     op->addTag(TAG_HTTP);
@@ -0,7 +0,7 @@
   }
   {
     OptionHandler* op(new UnitNumberOptionHandler(
-        PREF_MIN_SPLIT_SIZE, TEXT_MIN_SPLIT_SIZE, "20M", 1_m, 1_g, 'k'));
+        PREF_MIN_SPLIT_SIZE, TEXT_MIN_SPLIT_SIZE, "20M", 1, 1_g, 'k'));
     op->addTag(TAG_BASIC);
     op->addTag(TAG_FTP);
     op->addTag(TAG_HTTP);
@@ -0,7 +0,7 @@
   }
   {
     OptionHandler* op(new UnitNumberOptionHandler(
-        PREF_PIECE_LENGTH, TEXT_PIECE_LENGTH, "1M", 1_m, 1_g));
+        PREF_PIECE_LENGTH, TEXT_PIECE_LENGTH, "1M", 1, 1_g));
     op->addTag(TAG_ADVANCED);
     op->addTag(TAG_FTP);
     op->addTag(TAG_HTTP);
```

操作（Linux编译Windows平台）：

```bash
nano aria2.patch # 复制补丁文件
git clone https://github.com/aria2/aria2.git
cd aria2
patch --no-backup-if-mismatch -p0 -i ../aria2.patch
sudo docker build -f Dockerfile.mingw -t aria2-mingw .
cd ..
id=$(sudo docker create aria2-mingw)
sudo docker cp $id:/aria2/src/aria2c.exe aria2c.exe
sudo docker rm -v $id
unset id
sudo chmod 777 aria2c.exe
```
