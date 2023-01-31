## Pointpilar

## 框架：
-   点云转换为伪图像，进而通过2D卷积实现目标检测
    - Pillar Feature Net：将输入的点云转换为稀疏的Pseudo image
    - Backbone:处理Pseudo image得到高层的特征
    - Detection Head:检测和回归3D框
1. Stacked Pillars是如何变成Learned Features的？
- 上述的转换过程非常简单，可以表述为P*N*D(30000 x 20 x 9)->P*N*C(30000 x 20 x 64)-> P*C(30000*64)，对应的处理流程可参考代码如下，先是经过一个Linear+BN+ReLU，然后通过MaxPooling操作将每个Pillar中最大响应的点云提取出来

2.Learned Features 是如何变化为Pseudo Images?
- 生成伪图像：通过Scatter运算实现的。在PointPillars网络结构图中从Point cloud构造Stacked Pillars的
3. Loss函数
- 损失函数有三部分来组成，分别是定位Loss,分类Loss和朝向角Loss

## 数据处理

## 参数设置
- 学习率
- batch size


## Detection Head （SSD）

## Backbone

## 激活函数

## openpcdet 学习记录

1. git clone openpcdet代码
2. 下载kitti数据，软链接到openpcdet的data文件夹下

```bash
 ln -s /data/lidar_data/kitti_lidar/testing   ./data/kitti/ 
 ln -s /data/lidar_data/kitti_lidar/training  ./data/kitti/  
```
3. 执行脚本对数据进行处理
```bash
 python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml
 ```

> https://ad-gitlab.nioint.com/ad/envinfo/ehy/4d-radar-project/-/blob/user/xinyi/open_source_PCDet/docs/GETTING_STARTED.md

4. q