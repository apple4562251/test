## manifest for repo of agv5

### Get repo Tool
```
curl https://storage.googleapis.com/git-repo-downloads/repo-1 > ~/bin/repo 
chmod a+x ~/bin/repo 
```

### Get Internal Dev Source Code

```
python3 ~/bin/repo init -u http://192.168.100.50/CompalAGV5/agv5_manifest.git -m default.xml
  or
python3 ~/bin/repo init -u http://10.110.217.40/CompalAGV5/agv5_manifest.git -m compal_default.xml
repo sync
```

### Install dependencies before building the code

#### If you wanna run the AGV Simulation on your computer, you should run these following commands first

```
sudo apt-get update && apt-get install -y ros-kinetic-desktop-full=1.3.1-0*
sudo apt-get update --fix-missing
sudo apt-get install \
     ros-kinetic-serial -y \
     ros-kinetic-bfl -y \
     ros-kinetic-urg-c -y \
     ros-kinetic-laser-proc -y \
     ros-kinetic-move-base-msgs -y \
     ros-kinetic-ecl -y \
     libsdl-image1.2-dev -y \
     ros-kinetic-ros-control ros-kinetic-ros-controllers \
     ros-kinetic-gazebo-ros-control ros-kinetic-laser-proc \
     ros-kinetic-hector-gazebo-plugins -y \
     ros-kinetic-navigation-layers -y \
     ros-kinetic-hector-models -y \
     ros-kinetic-robot-pose-publisher -y \
     ros-kinetic-tf2-web-republisher -y \
     ros-kinetic-rosapi -y \
     ros-kinetic-rosauth -y \
     ros-kinetic-rosbridge-library -y \
     python-wstool -y \
     python-pandas -y
sudo apt-get install vim -y
sudo ln -s /usr/include/gazebo-7/gazebo/ /usr/include/gazebo
sudo ln -s /usr/include/sdformat-4.0/sdf/ /usr/include/sdf
sudo ln -s /usr/include/ignition/math2/ignition/math.hh /usr/include/ignition/math.hh
sudo ln -s /usr/include/ignition/math2/ignition/math /usr/include/ignition/math
curl -sL https://deb.nodesource.com/setup_6.x | bash - && \
    apt-get install nodejs iputils-ping -y
```

#### If you wanna use the Cartographer Slam on your computer, you should run these following commands first

```
sudo apt-get install -y \
     libgoogle-glog-dev \
     libsuitesparse-dev \
     python-sphinx \
     libcairo2-dev \
     liblua5.2-dev \
     libatlas-base-dev

git clone http://10.110.217.40/CompalRobotCommon/cartographer_deps.git
cd cartographer_deps
bash all.sh
```

## To build up all projects

First, `cd` to the workspace root directory, then insert the following commands

```
rosdep install --from-paths src --ignore-src -r -y
catkin_make
```

Once all the packages are compiled and linked, you are good to go!

## CI Setting
- You can integrate your code into CI runner Now.
- To do this, please add the '.gitlab-ci.yml' in the root of your repository.
- All you have to do is to change three variables: 'ROS_PKG_NAME', 'PKG_DIR_NAME' and 'PROJECT_DIR'
- [Tamplate](http://192.168.100.50/snippets/8)
- It will build in the runner server and show the build status for your code.
- Note: If your commit message contains [ci skip] or [skip ci], using any capitalization, the commit will be created but the jobs will be skipped.
- Note: [.gitlab-ci.yml DOC](https://docs.gitlab.com/ce/ci/yaml/#image-and-services)
