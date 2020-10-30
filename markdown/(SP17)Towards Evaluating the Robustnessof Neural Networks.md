# [(2017 SP) Towards Evaluating the Robustnessof Neural Networks](https://ieeexplore.ieee.org/abstract/document/7958570)

## 1.Summary
In this paper, the author demonstrated that the previously proposed methods abount ehancing the robustness neural networks are less effective by introducing three novel attack algorithms that are successful on both secured and unsecured nerworks with a probability of 100%.  
The paper only focused on the computer vision nerual network, but the method it proposed may be suitable for different AI problem too.  

## 2.Challenge
To perform the attack, we need to find an adversarial example based on the origin example.
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
###
## Strength

## Weakness
