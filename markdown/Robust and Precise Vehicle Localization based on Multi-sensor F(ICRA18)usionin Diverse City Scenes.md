# (2018 ICRA)Robust and Precise Vehicle Localization based on Multi-sensor Fusionin Diverse City Scenes
## 1 Summary
In this paper the author proposed a robust and precise global localization system based on multi-sensor fusion. Specifically, the author fused sensor output from IMU, LiDAR and GNSS. A Kalman-Filter based method is used to generate the final localization information. To improve the accuracy the author used both intensity and altitude information from LiDAR. The algorithm is proved to have centimeter-grade accuracy in complex urban environments such as high way and tunnel by deploying it on a fleet of autonomous vehicles. The paper is present by Baidu Apollo team.

## 2 Challenge
Compared to tradition environments faced by the previous Robotics, the urban environments has much more uncertainty. So the localization algorithm used in autonomous driving should deal with these uncertainty properly.  
Furthermore, the urban environment is complex and crowded, so the algorithm should get a centi-meter grade accuracy which is impossible for any single point algorithms.

## 3 Main Idea
A multi-sensor fusion method is used by the author to perform the localization process. The main idea behind it is that the GNSS, IMU and LiDAR sensor are complementary and can get benifits from each other. The overall architecture can be illustrated in the picture below.

![msf_arch](../images/wk4_arch.png)

### 3.1 LiDAR Based Localization
The goal of LiDAR based Localization is to generate the position information using an online point cloud and a pre-built HD map. The position information is a 4 dimensional vector (x, y, a, h) that contains the horizontal location, altitude and heading angle.  
The HD map used in this paper contains both the intensity information and altitude information, so the altitude of a robot can be obtained directly if the horizontal coordinate is determined. As a result, we just need to calculate x, y and h.  
We can use an exhaustive search to get the optimal result, but for the sake of efficiency, a separate h estimation step is performed before the estimation of x and y. Specifically, the method applies a Lucas-Kanade algorithm that searches for best match between two images using the gradient descent.  
In the estimation of horizontal localization, the author used a horizontal filter method. The horizontal localization decompose the state space to finite regions and generate the probability for each region. The a exhaustive seaerch is used to get the optimal result.
### 3.2 GNSS Based Localization
As with many popular GNSS based localization methods, the author used RTK(Real Time Kinematic) algorithm to determine the position of the vehicle. To improve the accuracy of RTK, it used all available GNSS infrastructures, GPS, BeiDou, GLONASS to get the final result. However, when driving throw places with severe multipath and signal blockage, the RTK's ambiguity resolution may be failed. In these situations, other sensors' observations are used to mtigate the uncertainty.

### 3.3 Sensor Fusion
In the fusion framework, an error-state Kalman filter is applied to fuse the localization measurements by LiDAR and GNSS with IMU. The SINS Kinematic Inertial Navagation System is used to generate the position, velocity, and attitude within the IMU.   
Because the SINS error grows over time, the author used an error-state Kalman filter to estimate the error of SINS and correct it.  
To achieve a high success rate, the measurement delay and disorder must be taken into consideration. The sensor fusion framework maintains two independent filters and used the difference between the tow filters to determine whether there is a disorder or not.  
