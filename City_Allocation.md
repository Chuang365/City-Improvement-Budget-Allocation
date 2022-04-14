<font color=black size=4 face=雅黑>In the first week, we illustrated the mathematic model and went through the two main goals
    we want to achieve : Best to optimize the available traffic flow and minimize the infrastructure budget. To solve such a problem, the bilevel problem solving algorithms are introduced. We went through a sample paper of bilevel solving last week, which with essential idea of reformulating the equation of 2nd-layer optimzition into a non-linear MIP. The mathematical model is

\begin{equation} 
\mathop{min}_{x} Z_f(x) = \sum_{j \in J}C_jX_j
\end{equation}

\begin{equation}
\downarrow
\end{equation}
$\newline$

\begin{equation} 
\sum_{j \in J}a_{kj}X_j + \sum_{i \in I}a_{ki}^{'}y_{i} \leq b_{k}  \\
\sum_{j \in J}C_jX_j \leq \sum_{k \in K}u_kb_k - \sum_{k \in K}\sum_{i \in I}a_{ki}^{'}u_ky_i \\
\sum_{k \in K}a_{kj}u_{k} \leq c_{j}  \qquad  \forall j \in J \\
u_{k} \leq 0 \qquad \forall k \in K
\end{equation}


$\newline$
\begin{equation}
\downarrow
\end{equation}
$\newline$


\begin{equation} 
\sum_{j \in J}a_{kj}X_j + \sum_{i \in I}a_{ki}^{'}y_{i} \leq b_{k}  \\
\sum_{j \in J}C_jX_j \leq \sum_{k \in K}u_kb_k - \sum_{k \in K}\sum_{i \in I}a_{ki}^{'}u_ky_i \\
u_{ki} \leq u_{k} + M(1-y_{i}) \qquad \forall i \in I, \forall k \in K\\
u_{ki} \geq u_{k} \qquad \forall i \in I, \forall k \in K\\
u_{ki} \geq u_{k} -My_{i} \qquad \forall i \in I, \forall k \in K\\
u_{ki} \leq 0 \qquad \forall i \in I, \forall k \in K
\end{equation}

<font color=black size=4 face=雅黑>These constraints ensure that the newly introduced decision variable $u_{ki}$ take the value 0 if $y_{i} = 0$ and $u_k$ if $y_{i} = 1$. The M being a very large positive number. Then we use Benders Decomposition decompese the original situation into the dual slave problem and the master problem. By iteratively find optimality cuts from the dual slave problem and add them into the master problem. The original Problem is much easier to solve.

<font color=black size=4 face=雅黑> The iteration to solve is shown as follow:
* 1 Initialization : upper bound $UBD = \infty $; set $y^*$ to any feasible solution of $y$
* 2 Solve the slave problem for $y = y^*$, let $z_s$ be the current optimal value regrading the slave problem and set the upper bound $UBD = min\{UBD,z_s\}$ 
    * If it's bounded, add an optimality cut to the master problem 
    * Else add a feasibility cut to the master problem 
$\newline$ break when $UBD - z_M < tolerance$

 <font color=black size=4 face=雅黑> The dual slave problem is :
    $$\mathop{max}_{\alpha,\beta,\gamma,\delta} \sum_{k \in K}(b_k - \sum_{i \in I}a_{ki}^{'}y_{i}^{*})\cdot \alpha_k + \sum_{j \in J}c_j\gamma_j + \sum_{i \in I}\sum_{k \in K}(M(1-y_{i}^{*})\delta_{1ki} - My_{i}^{*}\delta_{3ki})$$ 
    $\newline$
    $s.t$ $$\sum_{k \in K}a_{kj}\alpha_{k} + c_{j}\beta \leq f_{j}$$ \
    $$-b_{k}\beta + \sum_{j \in J}a_{kj}\gamma_j - \sum_{i \in I}(\delta_{1ki}+\delta_{2ki}) \geq 0 \qquad \forall k \in K$$ \
    $$\alpha_{ki}^{'}\beta + \delta_{1ki} + \delta_{2ki} + \delta_{3ki} \geq 0$$ \
    $$\alpha_{k} \leq 0$$ \
    $$\beta \leq 0$$ \
    $$\gamma_{j} \leq 0 \qquad \forall j \in J\ $$ \
    $$\delta_{1ki} \leq 0 \qquad \forall i\in I, k \in K$$ \
    $$\delta_{2ki},\delta_{3ki} \geq 0 \qquad \forall i\in I, k \in K$$ \
    
    
    
    

 <font color=black size=4 face=雅黑> If this problem is feasible with a solution $\alpha^{*}$; $\beta^{*}$; $\gamma^{*}$ and $\delta^{*}$, we add an optimality cut: $\newline$
    $$\sum_{i \in I}f_{i}^{'}y_{i} + \sum_{k \in K}(b_k - \sum_{i \in I}a_{ki}^{'}y_i)\cdot \alpha_{k}^{*} + \sum_{j \in J}c_j\gamma_{j}^{*} + \sum_{i \in I}\sum{k \in K}(M(1-y_i)\delta_{1ki}^{*} - My_i\delta_{3ki}^{*}) \leq z$$  
     If it is unbounded, a new constraint, which bounds the objective function with a Big-M $M_2$, is added to
the Slave Problem. $M_2$ has to be large enough such that no extreme point is cut off and only the extreme rays are bounded.
The Slave problem can then be solved by including this constraint: $\newline$
    $$\sum_{k \in K}(b_k - \sum_{i \in I}a_{ki}^{'}y_i)\cdot \alpha_{k} + \sum_{j \in J}c_j\gamma_{j} + \sum_{i \in I}\sum{k \in K}(M(1-y_i)\delta_{1ki} - My_i\delta_{3ki} \leq z$$
    The solution of this problem generates the following feasibility cut, which can be added to the Master Problem:
    $$\sum_{i \in I}f_{i}^{'}y_{i} + \sum_{k \in K}(b_k - \sum_{i \in I}a_{ki}^{'}y_i)\cdot \alpha_{k}^{*} + \sum_{j \in J}c_j\gamma_{j}^{*} + \sum_{i \in I}\sum{k \in K}(M(1-y_i)\delta_{1ki}^{*} - My_i\delta_{3ki}^{*}) \leq 0$$ 
    This is how we transform the original bilevel question to a singe-level optimization question

<font color=black size=4 face=雅黑> Let $C_o$ be the set of solutions ($\alpha^{*}$; $\gamma^{*}$; $\delta^{*}$) of optimality cuts and $C_F$ be the set of solutions ($\alpha^{*}$; $\gamma^{*}$; $\delta^{*}$) of feasibility cuts. In each iteration of the BD, a cut based on the solution of the Slave Problem is added to the respective set. Then, the corresponding Master Problem is defined as follows: $\newline$
    $$min z =  \sum_{i \in I}f_{i}^{'}y_{i} + \sum_{k \in K}(b_k - \sum_{i \in I}a_{ki}^{'}y_i)\cdot \alpha_{k}^{*} + \sum_{j \in J}c_j\gamma_{j}^{*} + \sum_{i \in I}\sum{k \in K}(M(1-y_i)\delta_{1ki}^{*} - My_i\delta_{3ki}^{*}) \qquad (\alpha^{*}; \gamma^{*}; \delta^{*}) \in C_o$$
    $$0 \geq \sum_{k \in K}(b_k - \sum_{i \in I}a_{ki}^{'}y_i)\cdot \alpha_{k} + \sum_{j \in J}c_j\gamma_{j} + \sum_{i \in I}\sum{k \in K}(M(1-y_i)\delta_{1ki} - My_i\delta_{3ki} \qquad (\alpha^{*}; \gamma^{*}; \delta^{*}) \in C_f$$

<font color=black size=4 face=雅黑> $\newline$ This week we have going through several papers of solving bilevel problem. This document aims at illustrating different methodologies of those papers. The plan of next week is that we are going to select the algorithm after discussion with the best performance and put into practice to see its stimulation results. 

<font color=black size=4 face=雅黑> The first paper is A neural network approach for solving linear bilevel programming problem. The original question is written as: 
    $\newline$
    $$ \mathop{min}_{x \in X} F(x,y) = c_1x + d_1y$$ \
    $s.t.$
    $$ \qquad A_1x + b_1y \leq b_1$$ \
    $$ \mathop{min}_{y \in Y} F(x,y) = c_2x + d_2y $$    
    $$s.t. \qquad A_2x + b_2y \leq b_2$$ 
    $\newline$
    Its core idea is firstly replace the lower level problem with its Kuhn-Tucker optimality conditions, transformed it to a one-level condition:
    $\newline$
    $$min \quad c_1x + d_1y$$\
    $s.t.$
    $$A_1x + B_1y \leq b_1$$\
    $$A_2x + B_2y \leq b_2$$\
    $$uB_2 - v = -d_2 $$\
    $$u(b_2 - A_2x - B_2y) + vy = 0$$
    $\newline$
    Then we reformulate it into a non-smooth equivalent reformulation, 2 more equivalents are added:
    $\newline$
    $$-2min(u,b_2-A_2x-B_2y) = 0$$ \
    $$-2min(v,y) = 0$$ \
    Note: Here the min operator is applied component-wise to the vectors. Such a model is still non-smooth, so we need to use smoothing method for the model, which is: 
    $\newline$
    Define a function: $$\psi_{\epsilon}(a,b) = \sqrt{(a-b)^2 + 4\epsilon^{2}} - (a+b)$$
    $\newline$
    Then the model can be written as:
    $\newline$
    $$min \quad c_1x + d_1y$$\
    $s.t.$
    $$A_1x + B_1y \leq b_1$$\
    $$uB_2 - v = -d_2 $$\
    $$\sqrt{[u_i - (b_2 - A_2x - B_2y)_i]^2 + 4\epsilon^2} - u_i - (b_2 - A_2x - B_2y)_i = 0 \qquad  i = 1,2 ... q$$ \
    $$\sqrt{(v_j - y_j)^2 + 4\epsilon^2} - v_j - y_j = 0 \qquad j = 1,2,...m $$
    $\newline$
    Then the paper introduced 2 notations here:
    $\newline$
    $$G(x,y,u,v) = \begin{bmatrix} A_1x + B_1y - b_1 \\ -x \end{bmatrix}$$ \
    $$H(x,y,u,v) = \begin{bmatrix} uB_2 - v + d_2 \\ \psi_{\epsilon}(u_i, (b_2 - A_2x - B_2y)_i) \quad i = 1,2...q \\ \psi_{\epsilon}(v_j,y_j)  \quad j = 1,...m\end{bmatrix}$$
    $\newline$
    let $x^{'}$ be $(x,y,u,v)$, the formula could be written as:
    $\newline$
    $$min f(x^{'}) = c_1x + d_1y$$ \
    $s.t.$
    $$G_l(x^{'}) \leq 0, \quad l=1 ... ,n+p$$ \
    $$H_k(x^{'}) = 0, \quad k = 1,...,2m+q$$ \
    Finally we could use neural network to sovle linear BLP problem, use Lagrange function to solve it.

<font color=black size=4 face=雅黑> 
    $\newline $The second paper is Using Karush-Kuhn-Tucker proximity measure for solving bilevel
optimization problems. The core idea is considering the pessimistic formulation, wherein the problem is solved considering the worst response from the follower out of its optimal set. The lower level problem can be replaced with its KKT conditions to obtain the reduced formulation. There are two difficulties associated with a KKT-based reduction. Firstly, a KKT-based
reduction can be done only for very special problems where the lower level satisfies certain regularity conditions.  Secondly, even when the formulation can be written, it can be difficult to handle; the Lagrangian constraint ($∇_yL(x, y, 𝜆) = 0$) and the complementary slackness conditions ($𝜆_jg_j(x, y) = 0$) are both equality constraints and may lead to non-convexities that are not straightforward to handle. 


```python

```
