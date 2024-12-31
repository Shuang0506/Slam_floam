```markdown
# **SLAM 教程：使用 ROS 2 搭建基于 LiDAR 的 SLAM 系统**

## **步骤 1：启动 LiDAR 驱动**
1. 确保已安装 rslidar 的 ROS 2 驱动程序 `rslidar_sdk`。
2. 启动 LiDAR 驱动程序：
   ```bash
   ros2 launch rslidar_sdk start.py
   ```
   **注意：**
   - 请确认 LiDAR 的 IP 地址和端口配置正确。
   - 可以通过修改配置文件（通常为 `params.yaml` 或类似文件）调整 LiDAR 的 IP 地址。

---

## **步骤 2：验证 LiDAR 数据**
1. 如果需要更改 LiDAR 的 IP 地址，请按照驱动文档修改对应配置文件。
2. 验证 LiDAR 数据是否正常发布：
   ```bash
   ros2 topic list
   ```
   查找 `/rslidar_points` 或类似主题，并检查点云消息：
   ```bash
   ros2 topic echo /rslidar_points
   ```

---

## **步骤 3：使用 ROS 2 bag 播放数据**
如果没有实物 LiDAR，可以使用记录好的 ROS 2 bag 数据来测试：
```bash
ros2 bag play rslidar_bag
```

---

## **步骤 4：启动 FLOAM SLAM 算法**
1. 启动 FLOAM：
   ```bash
   ros2 launch floam floam_launch.py
   ```
2. 如果需要更改订阅的点云主题：
   - 打开 `floam_launch.py` 文件，找到订阅点云的参数。
   - 修改订阅的点云主题名称为 `/rslidar_points` 或其他发布主题。

3. 调整 `params.yaml` 文件以优化 FLOAM 参数：
   - 更新算法的分辨率、点云降采样配置等。
   - 示例参数配置：
     ```yaml
     floam:
       lidar_topic: /rslidar_points
       max_range: 100.0
       min_range: 0.1
       voxel_size: 0.2
     ```

---

## **步骤 5：实时运行 SLAM**
1. 确认点云数据正常传输到 FLOAM。
2. 使用 RViz2 可视化 SLAM 过程：
   ```bash
   rviz2
   ```
   - 加载 FLOAM 的 RViz 配置文件，或手动添加点云和轨迹显示。

---

## **其他提示**
- 确保 `rslidar_sdk` 和 `floam` 的依赖已正确安装。
- 若遇到问题，可参考各自的官方文档或社区支持。

```
