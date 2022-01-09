# Cmake普通用户安装指南

官网下载压缩包

https://cmake.org/download/

tar -zvxf cmake-3.11.1.tar.gz

cd cmake-3.11.1

./bootstrap

./configure --prefix=安装目录（自己用户名下的目录）

make 

make install

给用户下的.bashrc添加下面信息：

`export PATH=安装目录/Cmake-xxx/bin:$PATH`

重启环境变量

source ~/.bashrc

cmake --version查看版本