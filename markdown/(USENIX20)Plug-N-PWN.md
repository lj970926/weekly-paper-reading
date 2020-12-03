# (2020 USENIX)Plug-N-Pwned: Comprehensive Vulnerability Analysis of OBD-II Dongles as A New Over-the-Air Attack Surface in Automotive IoT

## 1 Summary
A large number of OBD-II dongles have been deployed in  various automobiles, which provide the driver of the car capacities to monitor and operate the car remotely combined with the mobile apps. However, these widely used dongles and apps have quilt a lot vulnerabilities which can be exploited by the attacker to cause severe consequence, such as privacy leakage, emergency brake, etc. In this paper, the author devise an analyze tool to detect some common vulnerabilities in OBD-II dongles and conduct real experiments to prove the existence of these vulnerabilities.

## 2 Challenge
The main challenge in this work is the diversity of dongles. First, different dongles communicate with corresponding apps using different protocols, such as Wi-Fi or BLE. Second, the OBD-II port is not a strict standard and there are many customize implementation on them. So, it non-trivial to propose a generic method to identify the vulnerabilities.  
Another challenge comes from the fact that almost all messages provided by the dongles just perform some query request and these messages can't affect the vehicles significantly. So it's a requirement to devise undefined messages to perform attack and the analyze tool must also detect these situations.

## 3 Main Idea
### 3.1 Attack Model and Surface
In this paper, the author focused on the wireless OBD-II dongle, because it can offer attackers the ability to perform remote attack. The attack model is illustrated below:

![attack model](../images/wk6_attack_model.png)

First, the attacker tries to capture the broadcasting signals emitted by the target dongle, then he tries to build connection with it. Once the connection is  constructed, the attacker can send message to the dongle to try to get information of the vehicle or manipulating some aspects of it.  
The attack surface reside in the three phase of the three phases:

1. Broadcast Stage, in which the dongles emit identification information.
2. Connection Stage, with some possible verification steps.
3. Communicating Stage.

### 3.2 Vulnerability Analysis

The main goal of the analyze is trying our best to find vulnerabilities reside in the three phases. The author proposed a detection framework, DONGLE-SCOPE to perform the analyze. The workflow of the DONGLE-SCOPE is shown below.

![dongle scope workflow](../images/wk6_dongle_scope.png)

This framework test the OBD-II dongle on a vehicle and conduct static program analyze simultaneously. According to three phases, the framework can be divided into four main components. For each component in the dynamic analyze, a corresponding measurement is proposed to judge the existence of vulnerabilities.  
During the broadcasting stage, it collects the broadcast information. In the connection stage it examine not only the feasibility to build a connection but also the possibility to build multi connections while the user is using the dongle. In the communication stage, it tests whether predefined and undefined messages can pass the filter of the dongle and cause the desirable effect.

### 3.3 Experiment

The author collected 77 most popular OBD-II dongles on Amazon to conduct the experiment.
