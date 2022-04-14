# City-Improvement-Budget-Allocation

    This project is set up with Professor Xin,Wang from UW-Madison, aiming to optimiize City Improvement Budget Allocation problem through a toy sample first. 
    Using the bilevel optimization to solve basic traffic assignment model. 
    All the progress will be uploaded to this page to help people better track the development, which contexts but not limited to:

    1. Understanding basic traffic assignment model and setting up a mathematic model
    2. Learning newest and select one of the most effective bilevel algorithm method to solve such a model
    3. Establish a optimziation solution using Python to solve such a model
    4. Shareing ideas, new thoughts and questions.
  <font color=black size=4 face=雅黑> This week we have going through several papers of solving bilevel problem. This document aims at illustrating different methodologies of those papers. The plan of next week is that we are going to select the algorithm after discussion with the best performance and put into practice to see its stimulation results. 

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

