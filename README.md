# CommunityDetection-OptimizationApproach

This source codes, is an answer to question of clustering rc_task_2.csv points.

# 1 Solution Idea

To solve the given problem, we first need to be able to mathematically and precisely reformulate the problem. We can use various tools, each with its own advantages and disadvantages. The initial idea in this question is to solve a clustering problem. By solving this problem with the constraints we define, we can assign a number of points to each of the 5 groups for reconstruction. In clustering these points, an important point to consider is cost reduction. In the case of the given question, where the cost reduction is related to the movements between different points in each group, three factors become important:
1. The distance between different points within a group.
2. The distance between points of different groups.
3. The number of points in different groups.

To reduce the cost, we need to reduce the distance between points within a group while increasing the distance between points of different groups. At the same time, there should be a balance in the allocation of points to each group. 

In other words, we need to create a cost function that includes all three factors mentioned above and by optimizing it, we can reach the best possible solution. In the following three sections, we will briefly explain the mathematical criteria we use for each of these sections.

## 1 The distance between different points within a group

To assign a value to this criterion, we can calculate the Euclidean distance between each pair of points and then take the average. In this way, the smaller the value, on average, the closer the points within a group are to each other. For simplicity, we can calculate the centroid of the points and consider the criterion as the average distance of the points from the centroid. In this way, we are essentially calculating the variance of the points. We should try to minimize this value as much as possible.

## 2 The distance between points of different groups

Regarding this metric, similar to the previous metric, we have two approaches. The first approach is a more precise measurement, where the Euclidean distance between each pair of points belonging to different groups is calculated. The second approach is to measure the Euclidean distance between the average points of each group. By simultaneously computing both the variance and the distance between means, we can indirectly estimate the distance between points of different groups to some extent. It is essential to increase this metric, indicating a greater separation between the groups and a more distinct clustering result.

## 3 The number of points in different groups

In this case, we want the number of points in different groups to be relatively balanced. For example, we don't want 90% of the points to belong to one group and only 10% to the other groups. To achieve this, we can utilize the sum of squared counts of points in each group.

B = ∑_(i=1)^5▒〖|C_i |〗^2

If we assume our variables to be the |C_i |'s, subject to the constraint that their sum remains constant, this function is minimized when all the |C_i |'s are equal. Therefore, by defining L_3 as above, we can minimize it to approach an equal number of points in each group.

Hence, we need to construct a cost function that incorporates the three mentioned criteria. The implemented cost function is as follows:

L(α, β, γ) = -α.L_2 + β.L_1 + γ.L_3

By minimizing the above function, we can achieve the best compromise among the different parameters, reaching an optimal point that satisfies the assigned conditions, considering the assigned influence coefficients for each criterion.

# 2 Problem-solving approach

To solve this optimization problem, we follow the following approach. First, we assign a random group to each point. Then, we iterate over all points and update the cluster assignment of each point to minimize the cost. We determine which cluster the point belongs to by evaluating which assignment results in the lowest cost. We repeat this process several times for all points. The stopping criterion for the algorithm can be reaching the maximum number of iterations or no longer changing the minimum point. Since the problem size is relatively small, this method provides satisfactory output in terms of computational time. Additionally, the solution, as depicted in the provided file, is visually close to the intuitive clustering.

# 3 Parameter Selection

The optimized parameters that have led us to the desired output are as follows:

α = 0.001, β = 0.01, γ = 0.01

By increasing each of these variables, the focus is intensified on the corresponding dependent metric. If the number of points in each group is not significant, its value can be reduced (or set to zero). However, the involvement of the other two important factors is necessary. Since the constituent values of the cost function are not normalized or z-scored, it is not possible to provide a precise explanation or comparison of the parameter values. Through trial and error, one can obtain the optimal point based on the application.
