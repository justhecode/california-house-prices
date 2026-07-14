# deep-learning-notes

个人深度学习 / 机器学习学习笔记仓库。每个子目录是一个独立学习主题。

## 目录结构

```
deep-learning-notes/
├── california-house-prices/   # 加州房价预测（EDA → RF → PyTorch MLP）
├── Classify Leaves/           # 叶片分类（LeNet → AlexNet → VGG → ResNet）
└── README.md                  # 本文件
```

## 当前主题

| 目录 | 内容 | 状态 |
|------|------|------|
| [california-house-prices](./california-house-prices/) | 加州房价：Random Forest 基线 + PyTorch MLP | 进行中 |
| [Classify Leaves](./Classify%20Leaves/) | 叶片图像分类：经典 CNN 架构进阶 | 未开始（阶段 0） |

## 使用方式

进入对应子目录后运行脚本，例如：

```bash
cd california-house-prices
conda activate california-dl
python scripts/02_baseline.py
```

更详细的说明见各子目录内的 `README.md`。
