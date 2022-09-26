# Install ROS 2 Humble with Create 3 Messages on an Ubuntu 22.04 Machine

## Before you start
These directions should work on a machine natively running Ubuntu[^1] 22.04, as well as in a virtualized container within another operating system.
Note that there might be some network setup required if in a virtualized container; for example, RMWs seem to like running in a bridged network configuration rather than a NATted one.

These directions follow Open Robotics' official documentation on [Installing ROS 2 on Ubuntu Linux](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html), and more detailed information about what the commands below do can be found there.

## Step-by-step

1. If you haven't already, download and install [Ubuntu® 22.04 64-bit](https://releases.ubuntu.com/22.04/) onto your machine. You may choose either the desktop (for a GUI) or server (for console-only) install.

1. Once logged in, check to ensure that you are using a UTF-8 locale by typing

        echo $LANG
   and ensuring "UTF-8" is at the end of the returned string.

1. Ensure that the [Ubuntu Universe repository](https://help.ubuntu.com/community/Repositories/Ubuntu) is enabled by checking the output of this command:

        apt-cache policy | grep universe
which should output a line like

        500 http://us.archive.ubuntu.com/ubuntu jammy/universe amd64 Packages
          release v=22.04,o=Ubuntu,a=jammy,n=jammy,l=Ubuntu,c=universe,b=amd64
If it does not, execute the following:

        sudo apt update && sudo apt install software-properties-common && sudo add-apt-repository universe

1. Add the ROS 2[^2] apt repository, first by authorizing the Open Robotics GPG key

        sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
and then adding the repository to your computer's sources list

        echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

1. Make sure your other packages are up to date

        sudo apt update && sudo apt upgrade

1. And then install ROS 2. If you have a graphical user environment, use

        sudo apt install -y ros-humble-desktop
otherwise just use the base (barebones) install

        sudo apt install -y ros-humble-ros-base

1. We also recommend installing a few other packages:

        sudo apt install -y build-essential python3-colcon-common-extensions python3-rosdep ros-humble-rmw-cyclonedds-cpp #ros-humble-irobot-create-msgs

    !!! warning
        As of Sept. 22, 2022, the iRobot® Create® Platform interfaces (`irobot_create_msgs`) are not available via apt, but they should be available soon.
finally

        echo "source /usr/share/colcon_cd/function/colcon_cd.sh" >> ~/.bashrc
        echo "export _colcon_cd_root=/opt/ros/humble/" >> ~/.bashrc
        echo "source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash" >> ~/.bashrc
        echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

1. At this point, we recommend setting your default RMW (ROS 2 middleware). The RMW you set here has to match the RMW on your robot, which can be found from its Application Configuration page. More detail on RMW can be found [here](../xml-config). Right now, the Create® 3 robot supports `rmw_cyclonedds_cpp` and `rmw_fastrtps_cpp`. The default for Humble is `rmw_fastrtps_cpp`. Depending on your robot's RMW implementation, type one of the following:

        echo "export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp" >> ~/.bashrc
or

        echo "export RMW_IMPLEMENTATION=rmw_fastrtps_cpp" >> ~/.bashrc

1. Finally, either log out and log back in, or simply

        source ~/.bashrc

1. If both your computer and robot are on the same network, you should now be able to test things out with a `ros2 topic list`.
If this does not work, please refer to [ROS 2 Network Configuration](../xml-config/) for further configuration ideas.
A full Create® 3 API description can be found [here](../../api/ros2).

[^1]: Ubuntu is a registered trademark of Canonical Ltd.
[^2]: ROS 2 is governed by Open Robotics