# [(2018 ICRA)Robust and Precise Vehicle Localization based on Multi-sensor Fusion in Diverse City Scenes](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8461224)
## 1 Summary
In this paper the author proposed a robust and precise global localization system based on multi-sensor fusion. Specifically, the author fused sensor output from IMU, LiDAR and GNSS. A Kalman-Filter based method is used to generate the final localization information. To improve the accuracy the author used both intensity and altitude information from LiDAR. The algorithm is proved to have centimeter-grade accuracy in complex urban environments such as high way and tunnel by deploying it on a fleet of autonomous vehicles. The paper is present by Baidu Apollo team.

## 2 Challenge
Compared to tradition environments faced by the previous Robotics, the urban environments has much more uncertainty. So the localization algorithm used in autonomous driving should deal with these uncertainty properly.  
Furthermore, the urban environment is complex and crowded, so the algorithm should get a centimeter grade accuracy which is impossible for any single point algorithms.

## 3 Main Idea
A multi-sensor fusion method is used by the author to perform the localization process. The main idea behind it is that the GNSS, IMU and LiDAR sensor are complementary and can get benifits from each other. The overall architecture can be illustrated in the picture below.

![msf_arch](../images/wk4_arch.png)

### 3.1 LiDAR Based Localization
The goal of LiDAR based Localization is to generate the position information using an online point cloud and a pre-built HD map. The position information is a 4 dimensional vector (x, y, a, h) that contains the horizontal location, altitude and heading angle.  
The HD map used in this paper contains both the intensity information and altitude information, so the altitude of a robot can be obtained directly if the horizontal coordinate is determined. As a result, we just need to calculate x, y and h.  
We can use an exhaustive search to get the optimal result, but for the sake of efficiency, a separate h estimation step is performed before the estimation of x and y. Specifically, the method applies a Lucas-Kanade algorithm that searches for best match between two images using the gradient descent.  
In the estimation of horizontal localization, the author used a horizontal filter method. The horizontal localization decompose the state space to finite regions and generate the probability for each region. The a exhaustive search is used to get the optimal result.
### 3.2 GNSS Based Localization
As with many popular GNSS based localization methods, the author used RTK(Real Time Kinematic) algorithm to determine the position of the vehicle. To improve the accuracy of RTK, it used all available GNSS infrastructures, GPS, BeiDou, GLONASS to get the final result. However, when driving throw places with severe multi-path and signal blockage, the RTK's ambiguity resolution may be failed. In these situations, other sensors' observations are used to mitigate the uncertainty.

### 3.3 Sensor Fusion
In the fusion framework, an error-state Kalman filter is applied to fuse the localization measurements by LiDAR and GNSS with IMU. The SINS Kinematic Inertial Navigation System is used to generate the position, velocity, and attitude within the IMU.   
Because the SINS error grows over time, the author used an error-state Kalman filter to estimate the error of SINS and correct it.  
To achieve a high success rate, the measurement delay and disorder must be taken into consideration. The sensor fusion framework maintains two independent filters and used the difference between the tow filters to determine whether there is a disorder or not.  

## 4 Strength

1. Use not only the intensity of the LiDAR signals but also the altitude of them.
2. The multi-sensor fusion framework utilize all of the three sensors advantages ultimately.
3. The first algorithm  that can get centimeter-grade in complex urban environment.
## 5 weakness
1. Need more experiment to prove it can works well with some extreme weather.

# [(2018 USENIX)All Your GPS Are Belong To Us:Towards Stealthy Manipulation of Road Navigation Systems](https://www.usenix.org/system/files/conference/usenixsecurity18/sec18-zeng.pdf)
## 1 Solved Problem
In this paper, the author proposed an attack algorithm which attempts to manipulate the turn-by-turn navigation system to lead the drive to a wrong destination. In order to perform a low-cost attack, the author invented a novel GPS sproofing device with a total price of $223.  

![low cost sproofer](../images/wk4_low_cost_sproofer.png)

## 2 Main Idea
To make the attack unnoticeable. the author designed a searching algorithm which crafts GPS inputs to target device such that the triggered navigation instruction and displayed map in the car consistence with the real physical world. The author test the algorithm in real world and find it stealthy enough to most of the drivers.
## Highlights Worth Learning
1. The low-cost resolution of the GPS sproofer.
2. The method to perform a stealthy attack on GPS receivers.
   
# [(2019 IROS)An Automated Learning-Based Procedure for Large-scale Vehicle Dynamics Modeling on Baidu Apollo Platform](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8968102)
## 1 Solved Problem
The vehicle dynamic model plays an important role in the simulation of self-driving cars' control and planning module. A well-designed dynamic model can accelerate the development and iteration of algorithms used in these modules. However, tradition dynamic models are expressed using sophisticated analytical equations, which are unique for every specific  categories of autonomous vehicles. In this paper, the author proposed a learn-based algorithm that using machine-learning based method to design an general dynamic modeling procedure for all vehicles.
## 2 Main Idea
To solve problems in traditional modeling method, the method proposed contain three main steps:
* Data generation.
* Offline data pipeline.
* Online simulation.

![control in loop](../images/wk4_learn_based_arch.png)

In the data generation phase, the real vehicle driving data is collected and stored on the Apollo data platform. The data is then used to train the neural network model.   
In the model training phase, the extensive real road data is used to train the network model and there are some benchmarks to evaluate the performance of the model.  
In the online simulation phase, the trained model is used to perform the simulation which can effectively accelerate the development of control and planning algorithms.
## 3 Highlights Worth Learning
1. The insight that redeem the dynamic modeling problems as a learn-based problems.
2. Using RNN to model the dynamic of vehicles.