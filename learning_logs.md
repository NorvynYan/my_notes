# learning logs


## 下层pid控制器设计
1. 在platoon_mpc中添加一个通用的PIDController类
2. 在 PlatoonMPC 节点的初始化函数 __init__ 中，为每一辆车（leader, follower1, follower2）都创建一个独立的PID控制器实例。
3. 在主回调函数 mpc_callback 中，将MPC解算出的加速度 acc 转换为目标速度。
4. 将目标速度和当前速度的误差输入到对应车辆的PID控制器中。
5. 用PID的输出值来生成最终的油门/刹车指令，替换掉原来的 acc_to_throttle 函数。
