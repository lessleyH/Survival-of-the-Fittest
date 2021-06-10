Lessley Hernandez, Danielle Fang, and Haowen Liu,

<h1 align="center">Survival of the Fittest</h1>

Final project for multi-system robotics on foraging with ant optimization. 

Abstract— Animal behavior has inspired many algorithms and strategies in robotics work due to the efficiency of these systems honed by nature. In this work we explore ant foraging behavior of how ants are able to efficiently gather food through implicit coordination using the pheromone trails deposited as the ant travels. We use a virtual environment stage to efficiently model a large swarm of ants foraging in food in an unknown grid representation of a map. We then run experiments altering different environmental factors such as population of ants in the field and type of food source available to discover which scenario is able to maximize the amount of food foraged.

## Requirments 
- ROS -- tested on Melodic, but other versions may work.
- colcon -- used for building the application.
- stage_ros -- used for simulation

## Build
Once cloned in a ROS workspace, e.g., ros_workspace/src/, run the following commands to build it:

git clone: https://github.com/lessleyH/Survival-of-the-Fittest.git 

First terminal: 
Assuming running VNC-ROS 
```
docker-compose exec ros bash
source /opt/ros/melodic/setup.bash
roscore
```
In the second terminal:
```
docker-compose exec ros bash
source /opt/ros/melodic/setup.bash
cd ~/catkin_ws
catkin_make
echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
source ~/.bashrc
roslaunch ant_foraging ant_foraging.launch
```

In the third terminal:
```
docker-compose exec ros bash
source /opt/ros/melodic/setup.bash
rosrun ant_foraging controller
```

 ## Code Structure
 Main File Structure: 
```
Survival-of-the-Fittest
├─ CMakeLists.txt
├─ LICENSE
├─ README.md
├─ launch
│  └─ ant_foraging.launch
├─ nodes
│  ├─ ant
│  ├─ controller
│  └─ nest
├─ package.xml
├─ setup.py
├─ src
│  └─ ant_foraging
│     └─ __init__.py
├─ srv
│  ├─ AntStart.srv
│  ├─ EndRound.srv
│  ├─ GetNextLocations.srv
│  ├─ InitNest.srv
│  └─ UpdateNest.srv
├─ stage_config
│  ├─ maps
│  └─ worlds
└─ world
   └─ ant_world.world
```

## Nodes
### Ant
Main python class that deals with the movement of the ant bots.  

```python
#Parameters: positive float - sigma_1, positive float - sigma_2, positive float - mu 
#Purpose: This function initializes the class of ants 
#Returns: None
def __init__(self, sigma_1, signma_2, mu):

#Parameters: None
#Purpose: This function is to move the robot in the desired direction and check if it has food, removed, or is home
#Returns: None
def move(self):

#Parameters: None
#Purpose: This function to randomize direction of the ant and communicates with nest on possible steps
#Returns: goal type:Pos, that sets the new goal for movement of the ant 
def randomize_direction(self):

#Parameters: float - distance 
#Purpose: This function moves the ant forward and from simple motion
#Returns: distance type: float
def move_forward(self, distance):

#Parameters: rotation_angle - radians
#Purpose: This function rotates the ant and from simple motion
#Returns: None
def rotate_in_place(self, rotation_angle):

#Parameters: goal - Pos
#Purpose: This function handles the ros calls for the move function
#Returns: None
def move_helper(self, goal): 

#Parameters: goal - Pos
#Purpose: This function calculated euclidian distance from current position and goals
#Returns: distance - float
def euclidean_distance(self, goal):

#Parameters: goal -Pos
#Purpose: This function returns the angle to steer in 
#Returns: radian value of angle
def steering_angle(self, goal):

#Parameters: goal- Pos, constant - int
#Purpose: This function calculates the angular velocity of the robot
#Returns: returns the angular velocity - float 
def angular_vel(self, goal, constant=6):

#Parameters: data
#Purpose: Processing of odom position message and sets current position
#Returns: None
def _pos_callback(self, data):

#Parameters: None 
#Purpose: Stop the robot. Borrowed from Simple Motion
#Returns: None
def stop(self):

#Parameters: msg  
#Purpose: Processing of laser message. Borrowed from Simple Motion
#Returns: None
def _laser_callback(self, msg):

#Parameters: req
#Purpose: This function gets the callback message of the ant start service
#Returns: None
def _ant_start_callback(self, req):
```

### Controller 
Main python class that deals with the controller of the simulation that controls when the rounds starts and ends.This class also sends message to nest when round has ended and send food locations to nest. 
```python

#Parameters: rounds - int
#Purpose: This function initializes the class controller 
#Returns: None
def __init__(self, rounds):

#Parameters: req
#Purpose: This function starts the simulation 
#Returns: None
def start_simulation(self):

#Parameters:  num - int the amount of robots
#Purpose: This function is to reach out and signal when a round begins
#Returns: None
def ant_start_client(self, num):

#Parameters: num
#Purpose:  This function is to reach out and signal when a round ends
#Returns: None
def end_round_client(self, num):

#Parameters: foodType
#Purpose: This function initializes nest variables for food 
#Returns: None
def init_nest_variables(self, foodType):

#Parameters: req
#Purpose: This function checks the nest initilization 
#Returns: None
def handle_init_nest(self, req):
```
### Nest

```python
#Parameters: dimensions - int, i -int, j- int, foods- list 
#Purpose: This function initilizaes the enst class taking i nthe dimensions and foods
#Returns: None
def __init__(self, dimension, i, j, foods):

#Parameters: food_list -list
#Purpose: This function populates the food onto the grid
#Returns: None
def populate_food(self, food_list):

#Parameters: x- int, y - int
#Purpose: This is the map of the coordinates on the grid
#Returns: returns coordinates
def pose_to_grid(self, x, y):

#Parameters: i - int, j - int
#Purpose: This is the coordinates to the grid
#Returns: returns coordinates 
def grid_to_pose(self, i, j):

#Parameters: i - int, j - int, direction - int
#Purpose: This function gets the choices the ant can move to 
#Returns: None
def get_choices(self, i, j, direction):

#Parameters: x- int, y - int, hasFood - Boolean 
#Purpose: This function updates the nest and the food
#Returns: the results of these updates
def update_the_nest(self, x, y, hasFood):

#Parameters: req
#Purpose: Handles the call for next location to the ant 
#Returns: None
def handle_get_next_locations(self, req):

#Parameters: req
#Purpose: Handles the nest updates to the ant 
#Returns: None
def handle_nest_update(self, req):

#Parameters: req
#Purpose: This function checks for the end of the round 
#Returns: None
def handle_end_round(self, req):

#Parameters: None
#Purpose: This function evaporates the ants from the grid and map
#Returns: None
def evaporate(self):

```

### Demo 
![hippo](https://media.giphy.com/media/Er5tI8BcBehwpvJB4U/giphy.gif)

### Contribution 
- We all worked on reading, understanding and discussing the problem.
- Danielle and Lessley worked on the ant, controller, nest, launch, services, documentation, code clearning, and bug hunting.
- Haowen worked on the python demo and debugging the launch file.  

<details>
<summary><b>Refrences</b></summary>
[1] M. Dorigo, V. Maniezzo, and A. Colorni, “The Ant System: Optimization by a colony of cooperating agents.” IEEE Transactions on Systems, Man, and Cybernetics–Part B, vol. 26, no. 1, 1996.

[2] Yee Zi Cong and S. G. Ponnambalam, “Mobile Robot Path Planning using Ant Colony Optimization”, 2009 IEEE/ ASME International Conference on Advanced Intelligent Mechatronics, Suntec Convention and Exhibition Center, Singapore, July 14-17, 2009 

[3] Yee Zi Cong and S. G. Ponnambalam, “Mobile Robot Path Planning using Ant Colony Optimization”, 2009 IEEE/ ASME International Conference on Advanced Intelligent Mechatronics, Suntec Convention and Exhibition Center, Singapore, July 14-17, 2009 

[4] R. Rashid, N. Perumal, I. Elamvazuthi, M. K. Tageldeen, M. K. A. Ahamed Khan and S. Parasuraman, "Mobile robot path planning using Ant Colony Optimization," 2016 2nd IEEE International Symposium on Robotics and Manufacturing Automation (ROMA), 2016, pp. 1-6, doi: 10.1109/ROMA.2016.7847836.

[5] Song-Hiang Chia, Kuo-Lan Su, Jr-Hung Guo, Cheng-Yun Chung, “Ant Colony System Based Mobile Robot Path Planning”, 2010 Fourth International Conference on Genetic and Evolutionary Computing.

[6] R. Uriol and A. Moran, “Mobile robot path planning in complex environments using ant colony optimization algorithm,” in 2017 3rd International Conference on Control, Automation and Robotics (ICCAR), Apr. 2017, pp. 15–21. doi: 10.1109/ICCAR.2017.7942653.

[7] Michael Brand, Michael Masuda, Nicole Wehner, Xiao-Hua Yu, “Ant Colony Optimization Algorithm for Robot Path Planning”, 2010 International Conf

[8] Daniel Angus,” Solving a unique Shortest Path problem using Ant Colony Optimization”, Communicated by T.Baeck, 2005

[9] Krentz, Timothy, et al. “A Modified Ant Colony Optimization Algorithm for Implementation on Multi-Core Robots.” 2015 Swarm/Human Blended Intelligence Workshop (SHBI), IEEE, 2015, pp. 1–6. DOI.org (Crossref), doi:10.1109/SHBI.2015.7321683.

[10] Yogita Gigras, Kusum Gupta, “Artificial Intelligence in Robot Path Planning”, International Journal of Soft Computing and Engineering (IJSCE) ISSN: 2231-2307, Volume-2, Issue-2, May 2012 

[11] Buniyamin N., Sariff N., Wan Ngah W.A.J., Mohamad Z., “Robot global path planning overview and a variation of ant colony system algorithm”, International Journal of Mathematics and Computers In Simulation. 

[12] L. M. Gambardella, E. D. Taillard, and G. Agazzi, “MACS-VRPTW: A multiple ant colony system for vehicle routing problems with time windows,” in New Ideas in Optimization, D. Corne et al., Eds. McGraw Hill, London, UK, 1999, pp. 63–76.

[13] Ricard V Sol ́e, Eric Bonabeau, Jordi Delgado, Pau Fern ́andez, and JesusMar ́ın. Pattern formation and optimization in army ant raids. Artificial Life, 6(3):219–226, 2000.

[14] A. Häger and A. Torkkeli-Johansson, ‘Simulated evolution of food foraging strategies of army ants’, Dissertation, 2019

[15] Hoff, Nicholas R., et al. “Two Foraging Algorithms for Robot Swarms Using Only Local Communication.” 2010 IEEE International Conference on Robotics and Biomimetics, 2010, pp. 123–30. IEEE Xplore, doi:10.1109/ROBIO.2010.5723314.

[16]CS540 Project - Eric Lloyd. http://pages.cs.wisc.edu/~elloyd/cs540Project/eric/elloyd-ants.html. Accessed 14 May 2021. 

[17] “Local ant system for allocating robot swarms to time-constrained tasks,” Journal of Computational Science, vol. 31, pp. 33–44, Feb. 2019, doi: 10.1016/j.jocs.2018.12.012.

[18] “A novel foraging algorithm for swarm robotics based on virtual pheromones and neural network,” Applied Soft Computing, vol. 90, p. 106156, May 2020, doi: 10.1016/j.asoc.2020.106156.

[19] C. Dimidov, G. Oriolo, and V. Trianni, “Random walks in swarm robotics: An experiment with Kilobots,” in Lecture Notes in Computer Science (including subseries Lecture Notes in Artificial Intelligence and Lecture Notes in Bioinformatics), vol. 9882. Springer Verlag, 2016, pp. 185–196.

</details>
