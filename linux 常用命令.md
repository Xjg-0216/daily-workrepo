#### 1. cp
```shell
cp [-r] 源文件  目标文件
```

#### 2. 查看当前文件夹占用总大小
```shell
du -sh
```

#### 3. dpkg

```shell
sudo dpkg -i .deb  安装
sudo dpkg -l    查看所有安装的软件
sudo dpkg -r soft_name  卸载软件
```

#### 4. scp

从本地复制到远程

```shell
scp local_file remote_username@remote_ip:remote_folder

scp local_file remote_username@remote_ip:remote_file 指定了文件名
```

从远程复制到本地

```shell
scp remote_username@remote_ip:local_foler remote_file
```

#### conda创建虚拟环境，pip安装遇到问题

conda创建虚拟环境，激活进入虚拟环境后，用pip安装包可能会安装在`base`环境中，但是查看`which pip`发现确实在虚拟环境，此时安装时用
`python -m pip install ..`


#### conda 相关命令

```shell
#新建虚拟环境
conda create --name virtual_name python=3.x
#查看包
conda list
#进入虚拟环境
conda activate virtual_name
#退出虚拟环境
conda deactivate
#安装包
conda install package
#查看所有环境
conda info -e
#删除某个虚拟环境
conda remove --name virtual_name --all
```

#### 使用conda 安装时遇到以下问题
Collecting package metadata (current_repodata.json): done
Solving environment: failed with initial frozen solve. Retrying with flexible solve.
Solving environment: / failed with repodata from current_repodata.json, will retry with next repodata source.

解决：
```
conda update -n base conda
conda update --all
```

#### 关机

`sudo poweroff`


#### 统计文件夹中文件的个数

```
ls -l | grep "^-" | wc -l  (不包括子目录)
ls -lR | grep "^-"| wc -l   （包括子目录）
```

#### vscode删除整行代码
`shift + ctrl + k`



#### pip离线安装.tgz文件

````cobol
tar -zxvf scikit-learn-0.24.2.tar.gz
cd scikit-learn-0.24.2
pip install -e .
````

#### 查看Jetson系列产品JetPack的版本信息

`sudo apt-cache show nvidia-jetpack`

#### 命令行和vscode的区别

使用VSCode的`launch.json`配置文件与直接在命令行终端运行脚本的主要区别在于工作目录（working directory）的概念。在命令行终端中，您当前的工作目录通常是您启动终端时所在的目录，或者您可以使用`cd`命令切换到其他目录。当您在终端中运行脚本时，相对路径是相对于当前工作目录的。

在VSCode中，`launch.json`中的`"program"`字段通常设置为`${file}`，这意味着它会运行当前打开的文件。然而，VSCode的工作目录默认是打开的文件夹的根目录，而不是当前文件的目录。因此，当您在`launch.json`的`"args"`字段中使用相对路径时，这些路径是相对于VSCode打开的文件夹的根目录，而不是相对于脚本所在的目录。

如果您想在`launch.json`中使用相对路径并且希望它们相对于脚本所在的目录，您需要在`launch.json`中设置`"cwd"`字段（current working directory），如下所示：
```
"cwd": "${fileDirname}" // 设置当前工作目录为脚本所在的目录
```