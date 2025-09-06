## platoon—mpc

1. 设计控制器
	- mpc_solver
	- 输入：
	- 车辆id
	'leader', 'follower1','follower2'
	- 参考轨迹
	x_ref, v_ref, a_ref
	- 当前状态
	x_now, v_now, a_now


2. 生成参考轨迹
	- get_ref_trajectory
	- 输入：起点，终点，
	- 根据需要的运动效果生成轨迹，先从起点，速度为0加速到3，然后匀速行驶，最后减速到0停到终点
	- 输出每隔0.1s的一个[x_ref,v_ref,a_ref]



3. 虚拟leader根据参考轨迹与运动学模型生成自身运动姿态
	- virtual_leader
	- 输入：x_ref,v_ref,a_ref  = get_ref_trajectory()
	- 输出：pub_virtual_leader_states()



4. 获取mpc控制器输入
- 获取参考轨迹：pub_virtual_leader_states
	x_ref,v_ref,a_ref = pub_virtual_leader_states

- 获取自身状态：
	1. x_now,v_now    =     	              '/carla/leader/odometry_leader',
'/carla/follower1/odometry_follower1',
'/carla/follower2/odometry_follower2',	
	2. a_now = 
 '/carla/leader/imu_leader',
 '/carla/follower1/imu_follower1',
 '/carla/follower2/imu_follower2',

5. mpc_solver设计
	5.1  获取模型
	model = 

<!--stackedit_data:
eyJoaXN0b3J5IjpbMzkxMDM3ODAzXX0=
-->