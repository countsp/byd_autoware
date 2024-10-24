**规划：**

Autoware输出160个点组成的路径，发布于/planning/scenario_planning/trajectory中:

**泊车：**
![Screenshot from 2024-10-24 08-40-09](https://github.com/user-attachments/assets/514e86f2-c929-4853-a979-065ca82b84a0)

**行车：**
![Screenshot from 2024-10-24 08-40-32](https://github.com/user-attachments/assets/f76afa4e-a555-4f4e-b8f4-68422d507641)

**打印一帧路径图像的代码**
```
# show once trajectory
# 
#  #!/usr/bin/env python3

import rclpy
from rclpy.node import Node
from autoware_auto_planning_msgs.msg import Trajectory
import matplotlib.pyplot as plt

class TrajectorySubscriber(Node):
    def __init__(self):
        super().__init__('trajectory_subscriber')
        self.subscription = self.create_subscription(
            Trajectory,
            '/planning/scenario_planning/trajectory',
            self.listener_callback,
            10
        )
        self.subscription  # 防止未使用的变量被垃圾回收

    def listener_callback(self, msg):
        # 提取每个点的position信息
        x_positions = [point.pose.position.x for point in msg.points]
        y_positions = [point.pose.position.y for point in msg.points]
        
        # 打印收到的位置信息
        self.get_logger().info(f'Received Trajectory: {len(msg.points)} points')

        # 作图
        plt.figure()
        plt.plot(x_positions, y_positions, marker='o', linestyle='-')
        plt.title('Trajectory Position Plot')
        plt.xlabel('X Position (meters)')
        plt.ylabel('Y Position (meters)')
        plt.grid(True)
        plt.show()

        # 关闭节点以停止运行
        rclpy.shutdown()

def main(args=None):
    rclpy.init(args=args)

    trajectory_subscriber = TrajectorySubscriber()

    # 阻塞线程，直到接收到消息后关闭
    rclpy.spin(trajectory_subscriber)

if __name__ == '__main__':
    main()

```

**连续打印路径图像的代码**

```
# show continously

import rclpy
from rclpy.node import Node
from autoware_auto_planning_msgs.msg import Trajectory
import matplotlib.pyplot as plt

class TrajectorySubscriber(Node):
    def __init__(self):
        super().__init__('trajectory_subscriber')
        self.subscription = self.create_subscription(
            Trajectory,
            '/planning/scenario_planning/trajectory',
            self.listener_callback,
            10
        )
        self.subscription  # 防止未使用的变量被垃圾回收

        # 初始化图表
        self.fig, self.ax = plt.subplots()
        self.line, = self.ax.plot([], [], marker='o', linestyle='-')
        plt.title('Real-Time Trajectory Position Plot')
        plt.xlabel('X Position (meters)')
        plt.ylabel('Y Position (meters)')
        plt.grid(True)

        # 设置交互模式
        plt.ion()
        plt.show()

    def listener_callback(self, msg):
        # 提取每个点的position信息
        x_positions = [point.pose.position.x for point in msg.points]
        y_positions = [point.pose.position.y for point in msg.points]
        
        # 打印收到的位置信息
        self.get_logger().info(f'Received Trajectory: {len(msg.points)} points')

        # 更新图表数据
        self.line.set_xdata(x_positions)
        self.line.set_ydata(y_positions)

        # 动态调整图表边界
        self.ax.relim()
        self.ax.autoscale_view()

        # 刷新图表
        self.fig.canvas.draw()
        self.fig.canvas.flush_events()

def main(args=None):
    rclpy.init(args=args)


# show continously

import rclpy
from rclpy.node import Node
from autoware_auto_planning_msgs.msg import Trajectory
import matplotlib.pyplot as plt

class TrajectorySubscriber(Node):
    def __init__(self):
        super().__init__('trajectory_subscriber')
        self.subscription = self.create_subscription(
            Trajectory,
            '/planning/scenario_planning/trajectory',
            self.listener_callback,
            10
        )
        self.subscription  # 防止未使用的变量被垃圾回收

        # 初始化图表
        self.fig, self.ax = plt.subplots()
        self.line, = self.ax.plot([], [], marker='o', linestyle='-')
        plt.title('Real-Time Trajectory Position Plot')
        plt.xlabel('X Position (meters)')
        plt.ylabel('Y Position (meters)')
        plt.grid(True)

        # 设置交互模式
        plt.ion()
        plt.show()

    def listener_callback(self, msg):
        # 提取每个点的position信息
        x_positions = [point.pose.position.x for point in msg.points]
        y_positions = [point.pose.position.y for point in msg.points]
        
        # 打印收到的位置信息
        self.get_logger().info(f'Received Trajectory: {len(msg.points)} points')

        # 更新图表数据
        self.line.set_xdata(x_positions)
        self.line.set_ydata(y_positions)

        # 动态调整图表边界
        self.ax.relim()
        self.ax.autoscale_view()

        # 刷新图表
        self.fig.canvas.draw()
        self.fig.canvas.flush_events()

def main(args=None):
    rclpy.init(args=args)

    trajectory_subscriber = TrajectorySubscriber()

    # ROS 2 spin以保持节点运行
    rclpy.spin(trajectory_subscriber)

if __name__ == '__main__':
    main()

    trajectory_subscriber = TrajectorySubscriber()

    # ROS 2 spin以保持节点运行
    rclpy.spin(trajectory_subscriber)

if __name__ == '__main__':
    main()

```
