# (2020 USENIX)Plug-N-Pwned: Comprehensive Vulnerability Analysis of OBD-II Dongles as A New Over-the-Air Attack Surface in Automotive IoT

## 1 Summary
A large number of OBD-II dongles have been deployed in  various automobiles, which provide the driver of the car capacities to monitor and operate the car remotely combined with the mobile apps. However, these widely used dongles and apps have quilt a lot vulnerabilities which can be exploited by the attacker to cause severe consequence, such as privacy leakage, emergency brake, etc. In this paper, the author devise an analyze tool to detect some common vulnerabilities in OBD-II dongles and conduct real experiments to prove the existence of these vulnerabilities.

## 2 Challenge
The main challenge in this work is the diversity of dongles. First, different dongles communicate with corresponding apps using different protocols, such as Wi-Fi or BLE. Second, the OBD-II port is not a strict standard and there are many customize implementation on them.