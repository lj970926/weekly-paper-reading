# [(2017 SP) Towards Evaluating the Robustness of Neural Networks](https://arxiv.org/pdf/1608.04644.pdf?source=post_page---------------------------0)

## 1.Summary
In this paper, the author proposed three different attack methods to generate adversarial examples towards nerual network models under different distance metric. These methods defeat the state of art defense strategy which are thought to be robust previously.  
The author also proved that a high-confidence adversarial for one model can apply to another model too. Even in the situation that the latter is defensed using the state-of-art method.  
The author suggest that the methods in this paper show be a benchmark in future defense attempts.


## 2.Challenge
The main challenge in finding adversarial examples is the examples generated should be distinguished with the origin example as little as possible.  
Furthermore, there are already some defensive method that let all of the previous attack algorithm invalid. So new attack methods should be proved to work effectively even with these defensive method.
## 3.Main Idea
### 3.1.Targeted attack
Let $C(x)$ be the correct label of an input example x. Given an valid example $x$ we can find a similar sample $x'$ with a target $t$ and $t \neq C(x)$. What an attacker should do is  find an valid example that is as close as possible to the origin input under some distance metric. This can be demonstrated as the formulation below.
$$
min\quad D(x, x + \delta)
$$
$$ 
s.t.\quad C(x + \delta) = t 
$$
$$ 
x + \delta \in [0, 1] 
$$
$D$ is the distance metric. The main distance metric used in finding adversarial examples is $L_0$, $L_2$ and $L_\infty$ norm.
* $L_0$ distance: the number of indexes $i$ such that $x'_i \neq x_i'.$
* $L_2$ distance: the standard Euclidean distance between $x$ and $x'$.
* $L_\infty$ distance: the maximum change among all indexes:
$$ 
L_\infty = max(\lvert x_1 - x_1'\rvert,\cdots, \lvert x_n - x_n' \rvert) 
$$
### 3.2.Objective function
Because the opjective function is hard to train, the author covert the function to a different but equivalent form:
$$ 
min\quad D(x, x + \delta) + c \cdot f(x + \delta) 
$$
$$ 
s.t.\quad x + \delta \in [0, 1] 
$$
The function $f$ is defined to be $\leq0$ if and only if $C(x) = t$.  
The assumption here is that we can find a proper constant $c$ to make the optimal solution to the latter matches the optimal solution to the  former.  
There are many possible choices for $f$. With examniating on the real dataset, the author choose the follow function:
$$ 
f(x+\delta) = (max_{i \neq t}(Z(x + \delta)_i) - Z(x + \delta)_t)^+ 
$$
### 3.3.Box constraints
To ensure the modification yields a valid image, there is a constraint on $\delta$: we must have $0 \leq x_i + \delta_i \leq 1$. This is called "box constraint" in the field of optimization.  
The normal gradient descent algorithm can meet the requirement. Previously used solution is to modify the gradient descent.But these modifications work badly in the author's method.  
To train the model with unmodified gradient descent, the author choose to change the variable used to train the model.Specifically, instead of optimizing the variable $\delta$, the author defined a new variable $w$ and set:
$$ 
\delta = \frac{1}{2}(tanh(w) + 1) - x 
$$
Because tanh varies form -1 to 1, $x + \delta$ is promised to be within the valid range. Then the standard gradient descent can be used to find the adversarial example.
### 3.4.$L_2$ attack
Putting these ideas together, the $L_2$ attack can be expressed as:
$$ 
min\quad \lVert\frac{1}{2}(tanh(w) + 1) - x\rVert_2^2 + c \cdot f(\frac{1}{2}(tanh(w) + 1)) 
$$
Applying gradient descent to this objective ofthen results in a local minimum. To solve this problem, the author randomly choose a set of starting points which are close to the origin image.  
The follow image shows the result of the $L_2$ attack for each source/target pair. We can see that almost all attacks are visually indistinguishable from the origin digit.
![L2 attack result](../images/l2_result.png)
### 3.5.$L_0$ attack
Unlike the $L_2$ attack, the $L_0$ norm is not differentiable and therefore is ill-suited for gradient descent.  
The resolution adopted is to maintain a set of fixed pixels. The set is initially empty. At each step, perform $L_2$ attack on restricted pixels that are not in the fixed set and then compute the gradient of $f$ with respect to $\delta$. Let's denote it as $g$. Then, try to select a pixel with a minimum $g_i \cdot \delta_i$ and add it into the fixed set. This means to eliminate the pixel that contribute least to the attack. After a few steps, we can finally find a minimum subset of pixels required to generate an adversarial example.  
With a warm-start method, the $L_0$ attack can be perform as fast as the $L_2$ attack. The following image show the result of $L_0$ result for each of source/target pair.
![l0 attack result](../images/l0_result.png)
### 3.6.$L_\infty$ attack
As the $L_0$ attack above, the $L_2$ attack's objective function is indifferentiable too. Using a gradient descent directly may not get a good effect because $\delta$ will oscillate between two different points. The reason for this phenomenon is the $L_\infty$ metric only penalizes the largets $\delta_i$, leaving other $\delta_j$ unmodified even though they are big too.  
To solve this problem, the author first modify the objective function
$$
\min c \cdot f(x + \delta) + \sum_{i} [(\delta_i - \tau)^+]
$$

Using this function, we can penalize all pixels whose intensity are larger than a threshold.
The value of $\tau$ is set to 1 initially. Everytime when all $\delta_i$ is less than $\tau$, it is reduced by a factor of 0.9.
The attack result is shown below
![l inf result](../images/l_inf_result.png)
### 3.7.Attack evaluation
The author evaluate the three attack algorithms on both original models and models protected with distillation. The datasets used in evaluation are MNIST, CIFAR and ImageNet.
![eval res unsecured](../images/eval_res_unsecured.png)
![eval res secured](../images/eval_res_secured.png)
The results have shown that:
1. On unsecured models, all of the three attacks get a success rate of 100% and the mean distance is fewer when comparing to previous method.
2. On models protected by defensive distillation, all previous proposed methods fail to find a adversarial example. However the success rate of algorithms in this paper generate adversarial examples without any defeat.
### 3.8.Transferability
Transferability refers to the phenomenon that an adversarial example for a model will often be an adversarial example for a different model, even the two models are different and trained with different datasets.  
So defense methods must prove they can cut off this tranferability. The author trains a standard model, and use the adversarial examples for the standard model to attack other two models. One of them is another standard model and the other is a model protected by defensive distillation.  
The author also modify the loss function $f$:
$$
f(x') = \max(\max \{Z(x')_i: i \neq t\} - Z(x')_t, -k)
$$
the constant $k$ is used to depict the confidence of the generated adversarial examples. The bigger the constant is, the stronger that we believe that the examples generated will results in an misclassfication.  
The result of this experiment shows that for the three methods proposed, a transferability is always feasible on both of the two models. The only differnce is that the distilled model need a larger constant $k$.
## 4.Strength
1. The three algorithms proposed in this paper obtain a huge advance compared to the previous method. They successfully prove that the state-of-art defensive strategy still has very big limitions.
2. The author demostrate many concepts at a high level, giving the reader an intuitive understand on the mechanism on the attack and defense methods and why they success/fail.
## 5.Weakness
1. In this paper, the author constraints his focus only on the computer vision, which is just a part of the deep learning. Other fields such as NLP use models which is very different. What will happen when using this method in these different models is remained for further validation.
2. The speed of these attacks are typically slower that previous methods, which may constraints their apply in some real-time scenarios.
# [(2018 VTC-Fall)Detecting and Mitigating Spoofing Attack against an Automotive Radar](https://www.cs.binghamton.edu/~kang/vtc18-prateek.pdf)
## 1.Solved problem
* Show the vulnerabilities of PyCRA in the context of automotive radar systems. The method will turn off the rader periodically which can descrease the availability of rader system on autonomous vehicles.
* Propose STCR, a new defense method that uses the spatio-temporal challenge-response scheme.
* Using Matlab to evaluate and compare the accuracy and robustness of PyCRA and STCR.
## 2.Main idea
The PyCRA uses a method that periodically trun of the radar. The assume behind it is that the attacker can't discover this behavior and will continously emit sproofing signal. As a result, if the receiver detect the signal received is higher than a threahold between the turn-off period, this signal is deemed as the attack signal.  
Hower, this method may cause some potential disks in the autonomous driving scenario beacuse the vehicle should detect the obstacle in real time.  
The modification the author made is to use a spatio-temporal scheme that transmit radar signal toward randomly selected directions. If the signal received is from a different, it's considered to be the attack signal.
## 3.Highlights worth learning
* The insight to find potential problems in state-of-art attack methods
* use change in space to substitude the change in time domain.
# [(2018 CVPR)Robust Physical-World Attacks on Deep Learning Visual Classification](http://openaccess.thecvf.com/content_cvpr_2018/papers/Eykholt_Robust_Physical-World_Attacks_CVPR_2018_paper.pdf)
## 1.Solved problem
* The author introduce Robust Physical Perturbation($RP_2$) to generate physical perturbations for physical-world objects to cause the misclassification of nerual network models.
* The author proposed a evaluation methodology to study the effectiveness of physical perturbations in both lab and field scenarios.
## 2.Main Idea
The are two major categorys of pertubation attack methods. One is digital adversarial attack which is performed by change the intensities of pixels, the other is physical adversarial attack which try to change the real world object.  
The digit-only methods don't work well in real-world scenarios because the complex environment conditions. So in this paper the author proposed to perform an attack by add stickers to a real object. The author choose to add black stickers the stop signs alongside the road.  
To achieve this goal, the author first generating adversiral examples with a procedure similar to the digit-only-attack but with some consideration on some physical world challenges. Then, a mask is appliied to gurantee that all of the spoofed points are located inside the target sign. The author evalute their method in both lab and field scenario with differnt angles and distances to the stop sign and all of these tests show a excellent performance.
## 3.Highlights worth learning
* the use of the mask which makes it possible to constraint the generated sproofed points inside the object makes it possible to perturb the real object indeed.
