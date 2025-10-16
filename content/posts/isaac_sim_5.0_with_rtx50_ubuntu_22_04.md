+++

title = "Ubuntu 22.04上使用RTX50系显卡运行Isaac Sim 5.0"
sort_by = "date"
date = "2025-10-13"
insert_anchor_links = "right"
generate_feeds = true

[taxonomies]
tags=["Simulation", "Isaac Sim"]
categories=["Embodies Intelligent"]

[extra]
disqus_comment = true
toc = true # show table-of-contents
comment = true # enable comment
copy = true # show copy button in code block

+++

## 背景
### NVIDIA显卡渲染问题

在NVIDIA RTX 50系列的显卡上运行Isaac Sim4.5时，场景渲染会有以下噪声问题：

![blurry problem](/images/blurry_problem.png)

参考以下[BUG Issues](https://github.com/isaac-sim/IsaacLab/discussions/2869)

![rendering problem](/images/rendering_issues_nvidia.png)

以及NVIDA论坛对此问题的[discussions](https://forums.developer.nvidia.com/t/rtx5080-isaac-sim-4-5-ubuntu22-04-installed-error-on-cuda-compute-capability/326200)

![rendering issues](/images/discussion.png)

按照如下的建议，我们需要使用Isaac Sim5.0才能解决渲染模糊、雪花点的问题：

![rendering issues](/images/solutions.png)

### Python版本问题
由于我们需要使用Isaac Sim中的ROS2 Bridge同外部组件进行通信，但是Isaac Sim5.0所使用的Python版本是3.11而Ubuntu 22.04所自带的Python版本为3.10，在使用apt repository安装的二进制ROS包时会提示：

![python version issues](/images/python_311_problems.png)

因此，我们需要使用Python3.11的环境，对ROS源代码进行编译。

## 编译方法
### Conda Env的准备
推荐使用Miniconda，安装方法参考如下：

```bash
mkdir -p ~/miniconda3
wget  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
source ~/miniconda3/bin/activate
conda init --all
```

### Isaac Sim和Isaac Lab的安装
参考如下页面：
<iframe src="https://isaac-sim.github.io/IsaacLab/main/source/setup/installation/binaries_installation.html" style="width: 100%; height: 50vh; border: none;"></iframe>

在Installation Guide中，有两种方式创建虚拟环境，这里我们选择使用conda创建venv：

```bash
./isaaclab.sh --conda isaaclab_dev
./isaaclab.sh --install
```

创建完成后，执行如下命令：

```bash
conda config --add channels conda-forge
conda activate isaaclab_dev
conda install -c conda-forge libboost libboost-devel boost-cpp libboost-python libboost-python-devel
conda install -c conda-forge colcon-common-extensions
export LD_LIBRARY_PATH=<your_conda_env_path>/isaaclab_dev/lib/:$LD_LIBRARY_PATH
python -m pip install netifaces coverage lark hydra-core
```

### ROS2 Humble源代码获取
参考以下页面：
<iframe src="https://docs.ros.org/en/humble/Installation/Alternatives/Ubuntu-Development-Setup.html" style="width:100%; height: 50vh; border: none;"></iframe>

### 特殊说明
#### 跳过的package
Ubuntu22.04中QT的版本是5，有些包需要省略掉，执行colcon build时需要跳过以下package：

1. qt_gui_cpp
2. rqt_gui_cpp
3. qt_gui_core
4. rqt

#### 需要加入的package
由于我们需要对外发布体素(VoxelGrid)消息，而VoxelGrid消息属于nav2_msgs，因此需要执行以下命令获取navigation的包：

参考以下表格，在下执行`git clone` 命令，同时请按照extra operations中的说明执行动作：

| Package Name                | Repo URL                                                     | Branch                                  | Extra Operations                                             | Clone path |
| --------------------------- | ------------------------------------------------------------ | --------------------------------------- | ------------------------------------------------------------ | ---------- |
| ompl                        | https://github.com/ompl/ompl.git                             | main                                    | src/ros2/ompl内执行`git submodule update --init --recursive` 注释掉src/ros2/ompl/CMakeLists.txt中的add_subdirectory(test)和add_subdirectory(demo) | src/ros2   |
| navigation2                 | https://github.com/ros-navigation/navigation2.git            | humble                                  | 注释掉src/navigations/nav2_smac_planner/CMakeLists.txt中的add_subdirectory(test) | src        |
| bondcpp                     | https://github.com/ros/bond_core.git                         | ros2                                    |                                                              | src/ros2   |
| behaviortree_cpp_v3-release | https://github.com/BehaviorTree/behaviortree_cpp_v3-release.git | debian/humble/jammy/behaviortree_cpp_v3 |                                                              | src/ros2   |
| BehaviorTree                | https://github.com/BehaviorTree/BehaviorTree.CPP.git         | master                                  |                                                              | src/ros2   |
| diagnostics                 | https://github.com/ros/diagnostics.git                       | ros2                                    |                                                              | src/ros2   |
| angles                      | https://github.com/ros/angles.git                            | ros2                                    |                                                              | src/ros2   |
| cv_bridge                   | https://github.com/ros-perception/vision_opencv.git          | humble                                  |                                                              |            |

最后，**执行下述命令编译**：

```bash
colcon build --symlink-install --packages-skip qt_gui_cpp rqt_gui_cpp qt_gui_core rqt nav2_system_tests --cmake-args -DCMAKE_BUILD_TYPE=Release --install-base=/opt/ros/humble-311
```

编译完成后，执行以下命令：

```bash
source /opt/ros/humble-311/setup.bash
git clone https://github.com/Zhefan-Xu/isaac-go2-ros2.git
cd isaac-go2-ros2
python isaac_go2_ros2.py
```

界面应显示如下：

![run preview](https://raw.githubusercontent.com/Zhefan-Xu/isaac-go2-ros2/isaacsim-4.5/media/sim-demo1.gif)
