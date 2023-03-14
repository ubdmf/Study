## 深度学习基础
优化器简介：深度学习——优化器算法Optimizer详解(BGD、SGD、MBGD、Momentum、NAG、Adagrad、Adadelta、RMSprop、Adam) - 郭耀华 - 博客园

卷积核等基础概念： CNN基础知识——卷积（Convolution）、填充（Padding）、步长(Stride)

BN层： https://blog.csdn.net/weixin_42080490/article/details/108849715
激活函数：https://www.jiqizhixin.com/articles/2021-02-24-7
NMS:https://blog.csdn.net/jq_98/article/details/122837164?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-122837164-blog-103857298.pc_relevant_recovery_v2&spm=1001.2101.3001.4242.1&utm_relevant_index=3
2D.3D卷积：https://www.jiqizhixin.com/articles/2019-02-22-22
框架学习笔记：https://github.com/jjw-DL/OpenPCDet-Noted
pointpillar解读：3D点云 (Lidar)检测入门篇 : PointPillars PyTorch实现_3D视觉工坊的博客-CSDN博客

调参技巧：深度学习参数怎么调优，这12个trick告诉你        https://github.com/google-research/tuning_playbook（火）


VFE：多层的体素特征编码

## openpcdet框架
- 环境配置：
## Pointpilar

- 整个模型包括 VFE，MAP_TO_BEV，BACKBONE_2D，DENSE_HEAD，POST_PROCESSING这5个部分

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
- 超参数
    - 学习率
    - batch size
    - 优化器
    - 权重
    - epochs次数
    - 卷积核
    - layer number
    - feature map
    - layer stride
    - NMS_THRESH
    - voxel size
    - number point per voxel

- 网络结构优化（相关pointpilar优化论文，结构是如何调整的）

- 列出模型用到的超算子（算子是否能够优化）

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
3. 执行脚本对数据进行预处理（为了保证高效加载数据，把数据处理成字典格式）
```bash
 python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml
 ```

> https://ad-gitlab.nioint.com/ad/envinfo/ehy/4d-radar-project/-/blob/user/xinyi/open_source_PCDet/docs/GETTING_STARTED.md

4. 快速运行DEMO
```bash
python test.py --cfg_file ./cfgs/kitti_models/pointpillar.yaml --ckpt ../checkpoints/pointpillar_7728.pth
```
5. 运行test.py去测试和评估预训练好的模型
- python test.py --cfg_file ./cfgs/kitti_models/pointpillar.yaml  --batch_size 4  --ckpt ../checkpoints/pointpillar_7728.pth (给定batch_size)

- python test.py --cfg_file ./cfgs/kitti_models/pointpillar.yaml   --ckpt ../checkpoints/pointpillar_7728.pth (使用yaml里给定的batch_size)

6. 训练模型：
    - 目前，要训练PointPillar或SECOND或PartA2，--batch_size取决于训练GPU的数量，因为我们使用$ {BATCH_SIZE} = 4 * $ {NUM_GPUS}，即--batch_size 32来训练8个GPU
    - 可以选择添加其他命令行参数--batch_size ${BATCH_SIZE}并--epochs ${EPOCHS}指定首选参数。extra_tag表示储存路径的一个文件夹名，最好和训练的参数保持一致
6.1 单GPU训练
    - python train.py --cfg_file cfgs/kitti_models/pv_rcnn.yaml --batch_size 1 --workers 1 --epochs 10 （workers的含义？？）
    - python train.py --cfg_file cfgs/kitti_models/pointpillar.yaml --batch_size=3 --epochs=10 --extra_tag 'my_data_1'
6.12 多GPU训练
    - CUDA_VISIBLE_DEVICES=0,1,2,3 python -m torch.distributed.launch --nproc_per_node=4 train.py --launcher pytorch --batch_size 8 --extra_tag baseline --cfg_file cfgs/kitti_models/pointpillar.yaml
    - 其中 CUDA_VISIBLE_DEVICES=0,1,2,3 表明使用gpu 0,1,2,3
    - python -m torch.distributed.launch 使用torch.distributed.launch工具进行分布式训练
    - --nproc_per_node=4表明使用4个gpu
6.21 pointpilar配置文件以及可改参数：

6.22 train.py 解析
- parse_config() 给定所需参数
- build dataloader按照配置文件创建dataloader
- build_network()
(module_topology=['vfe', 'backbone_3d', 'map_to_bev_module', 'pfe',
            'backbone_2d', 'dense_head',  'point_head', 'roi_head']
build_networks是根据 self.module_topology 的设定逐个进行模块构建
- pointpilar->Detector3DTemplate->module_topology->module_topology[i].build_network
- forward函数的任务是需要把输入层，网络层，输出层链接起来，实现信息的前向传导，让损失函数调用backward。
- 训练模型。调用model.train（）
- 测试模型，调用model.eval()
 
7. 3D目标检测的评估标准
   - AP（平均精度）
        - 平均精度 (AP) 度量的想法是将精度-召回曲线内的信息压缩成一个数字，可以用来轻松地比较算法之间的关系
   - IOU（交并比）
     - 真实目标和检测到目标的重叠比
   - 平均精度 (mAP) Mean Average Precision (mAP)
        - 平均精度 (mAP) 度量的想法是计算各种 IoU 阈值的 AP 分数，然后从这些值计算平均值
   - 精确度和召回率
        - 精确度： 算法找到的对象实际上对应于真实对象的概率是多少？P=TP/（TP+FP）
        - 召回率： 探测器发现真实物体的概率是多少？为了计算检测到实际物体的概率，我们需要将实际检测到的数量除以实际检测和漏检的总和。该度量称为“召回” R = TP/（TP+FN）