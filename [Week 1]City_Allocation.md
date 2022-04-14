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


