# Classify Leaves — CNN 架构学习

用 [Kaggle Classify Leaves](https://www.kaggle.com/competitions/classify-leaves) 风格数据，按历史顺序亲手实现并理解：

**LeNet → AlexNet → VGG → ResNet**

目标不是刷榜，而是搞清楚：卷积怎么提特征、网络为什么越来越深、残差解决了什么问题。

## 数据概况

| 项目 | 内容 |
|------|------|
| 路径 | `data/classify-leaves/` |
| 训练 | `train.csv` ≈ 18353 张，`image` + `label` |
| 测试 | `test.csv` ≈ 8800 张（仅路径） |
| 类别 | **176** 种叶片 |
| 图片 | `data/classify-leaves/images/*.jpg` |

> `data/` 体积大，默认不提交到 Git（见根目录 `.gitignore`）。

## 学习路线（严格按阶段）

```
0 数据与训练脚手架
    ↓
1 LeNet          最小可用 CNN
    ↓
2 AlexNet        更大输入 + ReLU/Dropout
    ↓
3 VGG            更深的 3×3 堆叠
    ↓
4 ResNet         残差连接（可加迁移学习）
```

不要跳步：下一阶段应能复用上一阶段的 `Dataset` / 训练循环 / 评估。

### 阶段 0 — 数据与脚手架（先做）

自己要写清楚的东西：

1. 读 `train.csv`，统计类别数、每类样本量、是否不平衡  
2. 用 `Dataset` + `DataLoader` 读图（注意相对路径：csv 里是 `images/xxx.jpg`）  
3. `label` → 整数 id 的映射（以及 id → 类名，方便提交）  
4. train / val 划分（建议分层采样）  
5. 统一的「训练一步 / 验证一步 / 记 accuracy」骨架  
6. 图像尺寸、归一化：后面每个网络输入尺寸不同，transforms 做成可配置

建议脚本：`scripts/00_eda.py`、`scripts/dataset_common.py`（或你习惯的命名）。

### 阶段 1 — LeNet

| 要点 | 说明 |
|------|------|
| 学什么 | 卷积 → 池化 → 全连接；通道变化；为何需要展平 |
| 输入 | 建议 resize 到 **32×32**（或 28×28），灰度或 RGB 均可，先跑通 |
| 期望 | 训练能跑通、能画 loss；176 类上精度不会高，**正常** |
| 产出 | `scripts/01_lenet.py` + `models/lenet.py` |

核心问题（学完应能回答）：感受野大概在变大吗？参数主要在哪一层？

### 阶段 2 — AlexNet

| 要点 | 说明 |
|------|------|
| 学什么 | 更大输入、更深卷积栈、**ReLU**、**Dropout**、Local Response Norm（可了解、现代常省略） |
| 输入 | 常见 **224×224**（原版偏 227，跟 torchvision 一致用 224 即可） |
| 对比 | 同一训练流程下，相对 LeNet 精度与训练时间如何变化 |
| 产出 | `scripts/02_alexnet.py` + `models/alexnet.py` |

### 阶段 3 — VGG

| 要点 | 说明 |
|------|------|
| 学什么 | **多个 3×3** 代替大卷积核；统一「卷积堆叠 + 池化」的块化思维 |
| 变体 | 先实现 **VGG11 / VGG16** 之一即可（参数量很大，批大小可能要减小） |
| 对比 | 更深是否一定更好？验证集是否过拟合？ |
| 产出 | `scripts/03_vgg.py` + `models/vgg.py` |

### 阶段 4 — ResNet

| 要点 | 说明 |
|------|------|
| 学什么 | **残差块**、捷径连接、退化问题；BasicBlock vs Bottleneck（可只精简实现 ResNet18） |
| 两条线 | (A) 从头训练 ResNet18；(B) ImageNet 预训练 + 改最后一层做迁移（推荐作为收尾） |
| 对比 | 同深度下有无 shortcut 的训练曲线差异（可选小实验） |
| 产出 | `scripts/04_resnet.py` + `models/resnet.py` |

可选加分：测试集预测 → `submission.csv`（格式对齐 `sample_submission.csv`）。

## 建议目录（学到哪建到哪）

```
Classify Leaves/
├── README.md                 # 本文件
├── data/classify-leaves/     # 原始数据（本地）
├── scripts/                  # 可运行实验脚本
├── models/                   # 网络定义（按架构拆分）
└── outputs/                  # 权重、曲线、提交文件（gitignore）
```

## 练习约定

- 用 **PyTorch** 手写网络结构（阶段 1–3 禁止一上来 `torchvision.models.xxx` 当黑盒；ResNet 迁移学习阶段再用 pretrained）。  
- 每次换架构，尽量只改 `model` 与 `transforms`，训练循环保持稳定。  
- 记录一张简单对比表：参数量、输入尺寸、最佳 val accuracy、训练耗时。

## 环境

可复用加州房价环境，或新建 conda 环境（需 `torch`、`torchvision`、`pandas`、`Pillow` 等）。

```bash
cd "Classify Leaves"
conda activate california-dl   # 或你自己的环境名
```

## 当前进度

| 阶段 | 状态 |
|------|------|
| 0 数据与脚手架 | 未开始 |
| 1 LeNet | 未开始 |
| 2 AlexNet | 未开始 |
| 3 VGG | 未开始 |
| 4 ResNet | 未开始 |

准备好后从 **阶段 0** 开始即可。
