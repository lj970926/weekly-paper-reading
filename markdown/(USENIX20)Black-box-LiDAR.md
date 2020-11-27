# (2020 USENIX)Towards Robust LiDAR-based Perception in Autonomous Driving: General Black-box Adversarial Sensor Attack and Countermeasures
## 1 Summary
In autonomous driving cars, the LiDAR sensor plays pivotal role in the perception module. In this paper, the author proposed the first black-box LiDAR sproofing attack on state-of-art LiDAR sensor and the neural network model behind it. To achieve this goal, the author first analyzed the occlusion pattern based on previous study. After identifying this phenomenon, the author conduct the attack with a high success rates. The author also devised two effective countermeasures which don't need to modify the existing hardware.

## 2 Challenge
Successful LiDAR sproofing attack has been realized in previous study. However, previous work has an assumption that attackers must know exactly how the neural networks model behind the LiDAR are constructed, which lose generality and need to construct adversarial examples for every specific models. Currently, some methods to conduct black-box attack on camera have emerged, but these method need the attack to construct an identical model by themselves which is hardly to achieve.
## 3 Main Idea
### 3.1 Design-level Vulnerability
Unlike previous works that conduct sproofing attack blindly, the author first analyze the reason why previous work can be successful with such a little number of sproofed points.  
The author first identified two different scenarios where a physical car has much fewer points in the output of LiDAR.
* an occluded vehicle
* a distant vehicle
the picuture below show the two different scenarios.

![two scene](../images/wk5_twoscene.png)

The author made an assumption which is the central idea in this paper: the two categories of points cloud can achieve the same results without factors that cause the decrement of points. So, attackers can also achieve their goals by imitating these pattern.

### 3.2 Black-box Sprofing Attack
Based on the idea above, the author proposed a black-box sproofing attack algorithm that works well for all state-of-art perception model.  
First, the author used real-world physical traces of occluded or distant vehicles to perform the attack. Specifically, the author pick points clouds that conform the pattern above and do spatial transformation to make target points closer to the vehicle. With the assumption proposed above, the removed points clouds can also fool the perception model.  
To make the attack more flexible,  the author also proposed a method which generates sproofed points from scratch. That is, 











# Adversarial Objects Against LiDAR-Based AutonomousDriving Systems
## 1 Solved Problem
Deep neural network has been proved to be vulnerable to adversarial sample attacks. However, tradition gradient based algorithm in the image domain can't get a great result because of some natures of the 3D points cloud.  
In this paper, the author proposed the first attack method that generating spoofed LiDAR points using a render that mitigate the behavior of the laser scan. To evaluate the invented method, the author bring the adversarial object in reality using 3D print. The result shows that the attack is effective to state-of-art industrial autonomous driving platforms such as Baidu Apollo.
## 2 Main Idea
At first, a mesh of 3D object is created to be the input of the total pipeline. The mesh is processed by a renderer which simulate the lase beam on the surface of the object. After that, the object is transformed to a set of points in the final points cloud.  
The renderer proposed above is differentiable so that it can use optimization-based algorithm to get adversarial examples.  
After generating the spoofing object, the author used 3D printing technology to get the real object. The author then evaluated its performance in real-world cars and the result shows that it can foo the state-of-art LiDAR perception algorithm effectively.
## 3 Highlights worth learning
1. perform the real-world experiments
2. the idea to generating spoofed points using 3D modeling
3. the evaluation black box algorithm.

# (2019 ICCV) Fast point r-cnn
## 1 Solved Problem
State-of-art 3D object detection algorithms based on bird-eye view and voxel view are likely to loss information contained in the raw points cloud. However,  applying CNN directly on the 3D points cloud has been proved computational expensive.  
In this paper, the author proposed a novel two-stage neural network to try to get both high quality and efficiency.  
## 2 Main Idea

