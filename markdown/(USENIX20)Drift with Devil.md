# (2020 USENIX)Drift with Devil: Security of Multi-Sensor Fusion based Localization in High-Level Autonomous Driving under GPS Spoofing
## 1 Summary
In this paper, the author perform the first study about GPS sproofing attack upon Mutlti-Sensor-Fusion localization algorithms used in the state-of-art autonomous vehicles. To systematically understand the security the author first evaluated the upper-bound attack effectiveness and then used the take-over effect found to design a novel GPS sproofing attack method, FushionRipper. The author evaluated FusionRipper on real sensor traces and found that it can get a high success rates. With the observation that the profiling of parameters play an important role in the final effectiveness, the author also designed an offline method that can indentify attack parameters with over 80% success rates. The time cost for this method is at most half a day which is affordable for most scenarios.
## 2 Challenge
The MSF-based localization method used to be seemed as security to GPS sproofing attack, because the sensor fusion process will use the data from LIDAR and IMU to calibrate the result. So the attackers must manage to compromise the sensor fusion in localization.
## 3 Main Idea
### 3.1 Attack goal
In this paper, the author proposed two attack goals:
* Off-road attack
* Wrong-way attack  
![attack categories](../images/wk2_cate.png)  
The main ideas of both attacks are to generate a later deviation in the localization output, which will make the car deviate from the traffic lane. The difference between two attacks is the distance needed to achieve the goal:    
![distance needed](../images/wk2_atk_gal.png)

### 3.2 Upper-bound attack effectiveness and cause analyze
To find out the upper-bound attack effectiveness, the author evalate a exhaustive search for each attack window in both an actual sensor trace and synthesized sensor trace. Here are the results:  
![up bd res](../images/wk2_up_res.png)  
form this CDF figure, we can findout that all of the windows in the synthesized window fail to achieve either of the attack goals. The majorty can't either. However there are also some windows which get a maximum deviation above 2 meters. This result proves that there is the feasibility two perform successful GPS sproofing attack.  
The author indentified four potential factors which contribute to this phenomenon:
* Initial MSF state uncertainty($P_0$)
* LiDAR measurement uncertainty($R_1^{lidar}$)
* Difference between LiDAR position and the original MSF output without attack($\Delta_{lidar}$)
* IMU measurement($imu_1$)  
### 3.3  FusionRipper

