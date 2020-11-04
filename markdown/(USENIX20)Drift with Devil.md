# (2020 USENIX)Drift with Devil: Security of Multi-Sensor Fusion based Localization in High-Level Autonomous Driving under GPS Spoofing
## 1 Summary
In this paper, the author perform the first study about GPS sproofing attack upon Mutlti-Sensor-Fusion localization algorithms used in the state-of-art autonomous vehicles. To systematically understand the security the author first evaluated the upper-bound attack effectiveness and then used the take-over effect found to design a novel GPS sproofing attack method, FushionRipper. The author evaluated FusionRipper on real sensor traces and found that it can get a high success rates. With the observation that the profiling of parameters play an important role in the final effectiveness, the author also designed an offline method that can indentify attack parameters with over 80% success rates. The time cost for this method is at most half a day which is affordable for most scenarios.
## 2 Challenge
The MSF-based localization method used to be seemed as security to GPS sproofing attack, because the sensor fusion process will use the data from LIDAR and IMU to calibrate the result. So the attackers must manage to compromise the sensor fusion in localization.
## Main Idea


