# Survival-of-the-Fittest


Commands

Assuming running VNC-ROS

Tab 1: 
docker-compose exec ros bash

source /opt/ros/melodic/setup.bash

roscore

Tab 2: 

docker-compose exec ros bash

source /opt/ros/melodic/setup.bash

cd ~/catkin_ws

catkin_make

echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc

source ~/.bashrc

rosrun stage_ros stageros $(rospack find ant_foraging)/world/ant_world.world

Tab 3:

docker-compose exec ros bash

source /opt/ros/melodic/setup.bash

rosrun ant_foraging controller
