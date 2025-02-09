
# windows11使用pycharm连接wsl2开发基于poetry的python项目


背景：公司开发的python项目用到了某个只提供了Linux版本的包，遂研究了一番如何在windows环境下进行开发。


1. windows安装 [wsl2](https://github.com "wsl2")
2. 进入到wsl2中，安装对应的python版本，建议使用pyenv，下面以3\.10\.14版本为例子。 pyenv安装方法可以参考[这里](https://github.com "这里"):[FlowerCloud机场](https://yunbeijia.com)


* 安装python之前确认安装好下面的库，不然安装完会缺失部分python基础库：



```
sudo apt-get update
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev

# 如果不幸已经安装了缺少库的python，可以根据下面的命令重新安装
pyenv uninstall -f 3.10.14
# 再次安装正确的版本，记得先运行上面的类库安装
pyenv install 3.10.14

```

3. 安装Pycharm Professional (只有专业版自带wsl连接)，下面提供两种方式运行项目。


* Pycharm提供了远程开发模式，ide界面跑在本地，代码运行在远程，可以直接连接到本机的wsl，目前是bate功能，启动起来比较麻烦，好处是可以直接配置wsl上面的poetry。


ps. 经过实践这种方式很耗资源，能直接把内存跑炸了 😃




---


* Pycharm还是跑在本地，指定解释器为wsl里面的python解释器，但是pycharm没有提供使用wsl里面的poetry的选择，需要一点手动操作，方法[来自](https://github.com "来自")：



```
# 进入项目所在目录，设置本地目录要用到的python版本  
pyenv local 3.10.14 

# 安装项目依赖
poetry install

# 查看目录解释器所在位置，设置pycharm项目解释器时要用到
# 输出例子：/home/tiger_linux/.cache/pypoetry/virtualenvs/serviceme-JDx4R2Ou-py3.10
poetry env info -p


# 如果项目已经创建了一个基于旧版本 Python 的虚拟环境，建议删除旧环境并创建一个新的：
# 列出所有虚拟环境
poetry env list
# 删除指定的虚拟环境（替换  为实际环境名称）
poetry env remove 
poetry env use $(pyenv which python)
poetry install


```

用pycharm正常打开项目，进入项目设置：
![image](https://img2024.cnblogs.com/blog/1586975/202501/1586975-20250104092009524-1933012499.png)


点进去之后，选择【Virtualenv 环境】并根据上面提到的【poetry env info \-p】命令获取到的解释器目录找到解释器，点击【确定】，设置完就大功告成。
![image](https://img2024.cnblogs.com/blog/1586975/202501/1586975-20250104092056888-1551415145.png)


ps. 上述设置完毕正常来说已经可以正常调试启动项目，可能会有部分包的问题，逐个排查即可， good luck 😃


