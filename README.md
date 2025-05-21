# 集群控制附加作业

## 作业内容

Swarm-Formation 是一个分布式的编队轨迹优化框架，用于密集环境中的编队飞行规划。本作业需要基于 Swarm-Formation，完成无人机编队飞行以及编队变换的仿真。

## 作业要求

1. 可以使用不限数量的无人机组成编队。
2. 无人机保持一定速度飞行，在飞行过程中依次变换出"S"，"Y"，"S"，"U"四种队形，变换过程中无人机需保持移动。每种队形需要保持6秒，允许起始队形为字母"S"。在队形保持期间需要体现障碍物避障能力。
3. 仿真场景已固定随机种子，请不要修改`map_generator`的代码以及`normal_hexagon.launch`文件中该节点的参数。

## 提交内容

1. 源代码
2. 飞行演示GIF
3. 报告：简述实现思路，与演示结果截图，不超过3页；

提交截止时间另行通知。

## 参考

1. [Distributed Swarm Trajectory Optimization for Formation Flight in Dense Environments](https://arxiv.org/abs/2109.07682), Lun Quan*, Longji Yin*, Chao Xu, and Fei Gao. Accepted in [ICRA2022](https://www.icra2022.org/).
2. [Robust and Efficient Trajectory Planning for Formation Flight in Dense Environments](https://arxiv.org/abs/2210.04048),Lun Quan, Longji Yin, Tingrui Zhang, Mingyang Wang, Ruilin Wang, Sheng Zhong, Zhou Xin, Yanjun Cao, Chao Xu and Fei Gao. Accepted for IEEE Transactions on Robotics

---

请阅读以下内容，以快速启动本次作业。



**Swarm-Formation** is a distributed swarm trajectory optimization framework for formation flight in dense environments.

- A differentiable graph-theory-based cost function that effectively describes the interaction topology of robots and quantifies the similarity distance between three-dimensional formations.
- A spatial-temporal optimization framework with a joint cost function that takes formation similarity, obstacle avoidance, and dynamic feasibility into account, which makes the swarm robots possess the ability to move in formation while avoiding obstacles.



## Table of Contents
* [About](#1-About)
* [Quick Start within 3 Minutes](#2-Quick-Start-within-3-Minutes)
* [Tips](#3-Tips)
* [Important updates](#4-Important-updates)
* [Acknowledgements](#5-Acknowledgements)
* [Licence](#6-Licence)
* [Maintenance](#7-Maintenance)

## 1. About
**Author**: [Lun Quan*](http://zju-fast.com/lun-quan/), [Longji Yin*](http://zju-fast.com/longji-yin/), [Chao Xu](http://zju-fast.com/research-group/chao-xu/), and [Fei Gao](http://zju-fast.com/research-group/fei-gao/), from [Fast-Lab](http://zju-fast.com/),Zhejiang University.

**Paper**: [Distributed Swarm Trajectory Optimization for Formation Flight in Dense Environments](https://arxiv.org/abs/2109.07682), Lun Quan*, Longji Yin*, Chao Xu, and Fei Gao. Accepted in [ICRA2022](https://www.icra2022.org/).

```
@article{quan2021distributed,
      title={Distributed Swarm Trajectory Optimization for Formation Flight in Dense Environments}, 
      author={Lun Quan and Longji Yin and Chao Xu and Fei Gao},
      journal={arXiv preprint arXiv:2109.07682},
      year={2021}
}
```
If our source code is used in your academic projects, please cite our paper. Thank you!

<a href="https://www.youtube.com/watch?v=lFumt0rJci4" target="blank">

  <p align="center">
    <img src="fig/post.jpg" width="600"/>
  </p>
</a>

Video Links: [Bilibili](https://www.bilibili.com/video/BV1qv41137Si?spm_id_from=333.999.0.0) (only for Mainland China) or [Youtube](https://www.youtube.com/watch?v=lFumt0rJci4).

## 2. Quick Start within 3 Minutes
Compiling tests passed on ubuntu 18.04 and 20.04 with ros installed. You can just execute the following commands one by one.
```
sudo apt-get install libarmadillo-dev
git clone https://github.com/SYSU-HILAB/Swarm-Control-Course-Addtional-Work.git
cd Swarm-Formation
catkin_make -j1
source devel/setup.bash
roslaunch ego_planner rviz.launch
```
Then open a new command window in the same workspace and execute the following commands one by one.
```
source devel/setup.bash
roslaunch ego_planner normal_hexagon.launch
```
Then use **"2D Nav Goal"** in rviz to publish the goal for swarm formation navigation. You need to specify the value of **flight_type** in run_in_sim.launch:
<p align = "center">
<img src="fig/set_goal_normal_hexagon.gif" width = "800" height = "363" border="2" />
</p>

**Now only two forms are supported to specify the target point.**
- flight_type = 2: use global waypoints
- flight_type = 3: use "2D Nav Goal" to select goal  

Finally, you can see a normal hexagon formation navigating in random forest map.

<p align = "center">
<img src="fig/normal_hexagon_2.gif" width = "800" height = "464" border="2" />
</p>

If you find this work useful or interesting, please kindly give us a star :star:, thanks!:grinning:
### 2.1 Quick Start with Docker
If your operating system doesn't support ROS noetic, docker is a great alternative.

First of all, you have to build the project and create an image like so:
```bash
## Assuimg you are in the correct project directory
make docker_build
```
After the image is created, copy and paste the following command to the terminal to run the image:

```bash
xhost +
make docker_run
```
Then execute the following command;

```
roslaunch ego_planner normal_hexagon.launch
```

## 3. Tips
1. We recommend developers to use **[rosmon](http://wiki.ros.org/rosmon)** to replace the **roslaunch**
- **Why we use rosmon?** : 
  It is very developer-friendly, especially for the development of multi-robots. 
- **How to use rosmon?** :
  [Install](http://wiki.ros.org/rosmon):
  ```
  sudo apt install ros-${ROS_DISTRO}-rosmon
  source /opt/ros/${ROS_DISTRO}/setup.bash # Needed to use the 'mon launch' shortcut
  ```
  Run the simple example of our project:
  ```
  source devel/setup.bash
  roslaunch ego_planner rviz.launch
  ```
  Then open a new command window in the same workspace and use **rosmon**:
  ```
  source devel/setup.bash
  mon launch ego_planner normal_hexagon.launch
  ```
  <p align = "center">
  <img src="fig/rosmon.jpg" width = "566" height = "379" border="2" />
  </p>

## 4. Important updates
- **May 9, 2022** -Add Interface: Publish target points through "2D Nav Goal" in rviz for swarm formation navigation.
- **April 12, 2022** - A distributed swarm formation optizamition framework is released. An example of normal hexagon formation navigation in random forest map is given.

## 5. Acknowledgements
**There are several important works which support this project:**
- [GCOPTER](https://github.com/ZJU-FAST-Lab/GCOPTER): An efficient and versatile multicopter trajectory optimizer built upon a novel sparse trajectory representation named [MINCO](https://arxiv.org/pdf/2103.00190v2.pdf).
- [LBFGS-Lite](https://github.com/ZJU-FAST-Lab/LBFGS-Lite): An Easy-to-Use Header-Only L-BFGS Solver.
- [EGO-Swarm](https://github.com/ZJU-FAST-Lab/ego-planner-swarm): A Fully Autonomous and Decentralized Quadrotor Swarm System in Cluttered Environments.

## 6. Licence
The source code is released under [GPLv3](https://www.gnu.org/licenses/) license.

## 7. Maintenance
We are still working on extending the proposed system and improving code reliability.

For any technical issues, please contact Lun Quan (lunquan@zju.edu.cn) or Fei Gao (fgaoaa@zju.edu.cn).

For commercial inquiries, please contact Fei Gao (fgaoaa@zju.edu.cn).
