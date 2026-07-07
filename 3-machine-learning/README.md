# 3 - 机器学习

范围：机器学习、Kaggle、数据分析、特征工程、模型训练、评估与提交。

写法：结论优先，代码最小化，记录能复用的流程。

## 3.0 总览

- 数据读取：路径、CSV、DataFrame、shape
- EDA：字段、缺失值、分布、异常值
- 特征工程：类别编码、数值缩放、时间特征、文本特征
- 验证：train/valid split、cross validation、metric
- 模型：baseline、调参、集成
- 提交：submission 格式、线下线上差异

## 3.1 Kaggle

- [3.1.1 读取 train.csv：os.path.join + pd.read_csv + len(train)](3.1-kaggle/3.1.1-read-train-csv.md)

## 3.99 固定起手

```python
import os
import pandas as pd

path = "/kaggle/input/<dataset>"
print(os.listdir(path))

train = pd.read_csv(os.path.join(path, "train.csv"))
test = pd.read_csv(os.path.join(path, "test.csv"))

print(train.shape, test.shape)
display(train.head())
```

先确认数据路径、行列数、字段，再做模型。
