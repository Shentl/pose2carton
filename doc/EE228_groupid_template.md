# Pose2Carton 

EE228 课程大作业，利用3D骨架控制3D卡通人物。



# Maya 环境配置

在windows环境下配置maya

- 从https://www.autodesk.com/education/edu-software 上下载Autodesk_Maya_2020_ML_Windows_64bit_di_en-US_setup_webinstall并运行

- 将~\Maya2020\bin加入到环境变量
- 安装pip
```
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -o get-pip.py
mayapy get-pip.py
```

- 安装numpy
```
mayapy -m pip install -i https://pypi.anaconda.org/carlkl/simple numpy
```
## Requirements
- Python environment 3.6
```
conda create -n pose2cartoon_env python=3.6
conda activate pose2cartoon_env
```
- python packages
```
pip install -r requirements.txt
```
- `winehq-stable` for cartoon face warping in Ubuntu (https://wiki.winehq.org/Ubuntu). Tested on Ubuntu16.04, wine==5.0.3.
```
sudo dpkg --add-architecture i386
wget -nc https://dl.winehq.org/wine-builds/winehq.key
sudo apt-key add winehq.key
sudo apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ xenial main'
sudo apt update
sudo apt install --install-recommends winehq-stable
```


# 匹配流程

- 在transfer.py中输入字典序列manual_model_to_smpl{}，里面是选用模型和给定的SMPL模型的关节点的匹配
- transfer.py首先调用transfer_one_sequence(infofile, seqfile）函数，，它有2个输入参数，infofile是filename.fbx对应的filename.txt文件，filename.txt文件一开始每一行是关节点信息，中间部分每一行是蒙皮信息，最后一部分是层次信息。而seqfile就是选择的info_seq_5.pkl文件， 其中保存的是所有帧的旋转矩阵。
- 调用transfer_one_sequence时首先提取逐帧旋转矩阵human_pose， 并将human_pose和filename.txt文件传入transfer_given_pose函数得到outmesh。
- 在transfer_given_pose函数中， 第一步先读取filename.txt中的层次结构，建立关节点名称到索引的映射，建立动力学结构。然后重新组织映射，确保每条运动链上索引递增。
- 第二步则是从filename.txt中解析出蒙皮权重， 表征为 J x V 的矩阵, J是关节点数，而V是顶点数，同时读取filename.txt中以“joints”开头的关节点信息， 从中解析出T-posed的卡通人物骨架，表现为J x 3的张量，即每一个关节点的空间坐标。
- 第三步是利用正向运动学将T姿势骨架转化为姿势化的角色骨架。
- 最后，根据第二步中解析得到的蒙皮权重和第三步得到的骨架生成混合权值，从骨架得到mesh。



# 新增脚本说明

如果你写了自己的脚本来处理数据或进行可视化，请在这里进行相关说明(如何使用等)； 如果没有，请忽略该模块。



# 项目结果

这里放置来自你最终匹配结果的截图， 如

![image](../img/pose2carton.png)





# 协议 
本项目在 Apache-2.0 协议下开源

所涉及代码及数据的最终解释权归倪冰冰老师课题组所有

Group xx
