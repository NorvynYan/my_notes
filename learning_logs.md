车辆运动学模型
  # 欧拉法离散化公式

## 连续时间模型
$$
\frac{d}{dt} 
\begin{bmatrix} 
x \\ 
v \\ 
a 
\end{bmatrix} 
= 
\begin{bmatrix} 
v \\ 
a \\ 
\frac{u - a}{\tau} 
\end{bmatrix}
$$

## 欧拉离散化公式
$$
X_{k+1} = X_k + \Delta t \cdot f(X_k, u_k)
$$

## 展开后的离散状态方程
$$
\begin{aligned}
x_{k+1} &= x_k + \Delta t \cdot v_k \\
v_{k+1} &= v_k + \Delta t \cdot a_k \\
a_{k+1} &= a_k + \Delta t \cdot \frac{u_k - a_k}{\tau}
\end{aligned}
$$

## 整理后的形式
$$
\begin{aligned}
x_{k+1} &= x_k + \Delta t \cdot v_k \\
v_{k+1} &= v_k + \Delta t \cdot a_k \\
a_{k+1} &= \left(1 - \frac{\Delta t}{\tau}\right) a_k + \frac{\Delta t}{\tau} u_k
\end{aligned}
$$

## 矩阵形式的状态空间方程
$$
\begin{bmatrix} 
x_{k+1} \\ 
v_{k+1} \\ 
a_{k+1} 
\end{bmatrix} 
= 
\begin{bmatrix} 
1 & \Delta t & 0 \\ 
0 & 1 & \Delta t \\ 
0 & 0 & 1 - \frac{\Delta t}{\tau} 
\end{bmatrix} 
\begin{bmatrix} 
x_k \\ 
v_k \\ 
a_k 
\end{bmatrix} 
+ 
\begin{bmatrix} 
0 \\ 
0 \\ 
\frac{\Delta t}{\tau} 
\end{bmatrix} 
u_k
$$

## 参数说明
- $x$: 车辆位置 (m)
- $v$: 车辆速度 (m/s)
- $a$: 车辆加速度 (m/s²)
- $u$: 控制输入 (期望加速度，m/s²)
- $\tau$: 执行器时间常数 (s)
- $\Delta t$: 离散时间步长 (s)



# learning logs


## 下层pid控制器设计
1. 在platoon_mpc中添加一个通用的PIDController类
2. 在 PlatoonMPC 节点的初始化函数 __init__ 中，为每一辆车（leader, follower1, follower2）都创建一个独立的PID控制器实例。
3. 在主回调函数 mpc_callback 中，将MPC解算出的加速度 acc 转换为目标速度。
4. 将目标速度和当前速度的误差输入到对应车辆的PID控制器中。
5. 用PID的输出值来生成最终的油门/刹车指令，替换掉原来的 acc_to_throttle 函数。
