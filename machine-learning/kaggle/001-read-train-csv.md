# 001 - Kaggle 中读取 train.csv：os、path、pd.read_csv 和 len(train)

## 典型代码

在 Kaggle notebook 或本地项目里，经常会看到这样的代码：

```python
import os
import pandas as pd

path = "/kaggle/input/your-competition-name"

train = pd.read_csv(os.path.join(path, "train.csv"))
print(len(train))
```

这几行代码做的事情是：找到 `train.csv` 文件，把它读成一个 pandas 的表格对象，然后打印训练集有多少行。

## 先理解 Kaggle 的数据目录

在 Kaggle notebook 中，比赛或数据集通常会挂载在：

```text
/kaggle/input/
```

例如某个比赛的数据可能长这样：

```text
/kaggle/input/titanic/
├── train.csv
├── test.csv
└── gender_submission.csv
```

这时 `path` 通常就是数据文件所在的文件夹：

```python
path = "/kaggle/input/titanic"
```

如果是在本地电脑，路径可能会变成：

```python
path = "./data"
```

或者：

```python
path = "/Users/your-name/projects/kaggle-titanic/data"
```

核心思想不变：`path` 指向数据文件所在的目录。

## `os` 是什么

`os` 是 Python 标准库里的一个模块，主要用来和操作系统交互，比如：

- 拼接文件路径
- 查看文件是否存在
- 创建文件夹
- 遍历目录
- 读取环境变量

使用前需要先导入：

```python
import os
```

在 Kaggle 数据读取里，最常用的是：

```python
os.path.join(...)
```

## 为什么要用 `os.path.join`

这行代码：

```python
os.path.join(path, "train.csv")
```

会把文件夹路径和文件名拼成完整路径。

如果：

```python
path = "/kaggle/input/titanic"
```

那么：

```python
os.path.join(path, "train.csv")
```

得到的是：

```text
/kaggle/input/titanic/train.csv
```

你也可以手写：

```python
"/kaggle/input/titanic/train.csv"
```

但不推荐一直手写，因为容易出这些问题：

- 少写 `/`
- 多写 `/`
- 换到 Windows、本地、服务器后路径分隔符不一致
- 后续代码不容易复用

更稳的写法是：

```python
train_path = os.path.join(path, "train.csv")
```

然后再读取：

```python
train = pd.read_csv(train_path)
```

这样代码含义更清楚。

## `pd.read_csv` 是什么

`pd` 通常是 pandas 的别名：

```python
import pandas as pd
```

pandas 是 Python 中最常用的数据分析库之一。它能把表格数据读成 `DataFrame`，也就是一个类似 Excel 表格的数据结构。

读取 CSV 文件：

```python
train = pd.read_csv(os.path.join(path, "train.csv"))
```

可以拆开理解：

```python
train_path = os.path.join(path, "train.csv")
train = pd.read_csv(train_path)
```

第一行得到文件路径，第二行把 CSV 文件读进内存。

读取成功后，`train` 就是一个 `DataFrame`。

## DataFrame 是什么

`DataFrame` 可以理解成 pandas 里的二维表格：

- 每一行是一条样本
- 每一列是一个字段或特征
- 既可以存数字，也可以存字符串、日期、类别等数据

比如 Titanic 训练集可能有：

```text
PassengerId | Survived | Pclass | Name | Sex | Age
```

其中：

- `Survived` 可能是标签，也就是要预测的目标
- `Pclass`、`Sex`、`Age` 可能是特征
- 每一行代表一名乘客

## `len(train)` 是什么

```python
print(len(train))
```

会打印 `train` 这个 DataFrame 的行数。

如果输出：

```text
891
```

意思是训练集中有 891 条样本。

这在 Kaggle 里很常用，因为读完数据后第一件事通常是确认：

- 数据有没有读成功
- 行数是不是符合预期
- 是否读错了文件
- 是否路径写错导致读取失败

## 更推荐的检查方式

只打印行数还不够。读完数据后，建议顺手检查这些内容：

```python
print(train.shape)
print(train.head())
print(train.columns)
```

### `train.shape`

```python
print(train.shape)
```

输出类似：

```text
(891, 12)
```

意思是：

- 891 行
- 12 列

它比 `len(train)` 信息更多。

### `train.head()`

```python
print(train.head())
```

会显示前 5 行数据。适合快速确认：

- 列名是否正常
- 数据是否乱码
- 分隔符是否读对
- 有没有明显异常

在 notebook 里也可以直接写：

```python
train.head()
```

显示效果通常更清楚。

### `train.columns`

```python
print(train.columns)
```

会打印所有列名。适合确认目标列、特征列和提交文件需要的列。

## 一份更完整的 Kaggle 起手代码

```python
import os
import pandas as pd

path = "/kaggle/input/titanic"

train_path = os.path.join(path, "train.csv")
test_path = os.path.join(path, "test.csv")

train = pd.read_csv(train_path)
test = pd.read_csv(test_path)

print("train rows:", len(train))
print("test rows:", len(test))
print("train shape:", train.shape)
print("test shape:", test.shape)

display(train.head())
display(test.head())
```

如果在普通 Python 脚本里运行，没有 `display` 时，可以改成：

```python
print(train.head())
print(test.head())
```

## 查看目录里有哪些文件

如果不确定数据文件名，可以先列出目录：

```python
import os

path = "/kaggle/input/titanic"

print(os.listdir(path))
```

输出可能是：

```text
['train.csv', 'test.csv', 'gender_submission.csv']
```

如果 Kaggle 的 input 下面有多个数据集，可以先看：

```python
print(os.listdir("/kaggle/input"))
```

再进入对应目录。

## 常见错误

### 1. 路径写错

错误示例：

```python
path = "/kaggle/input/titanic/train.csv"
train = pd.read_csv(os.path.join(path, "train.csv"))
```

这会拼出：

```text
/kaggle/input/titanic/train.csv/train.csv
```

问题在于：`path` 应该是目录，不应该已经包含 `train.csv`。

正确写法：

```python
path = "/kaggle/input/titanic"
train = pd.read_csv(os.path.join(path, "train.csv"))
```

### 2. 文件名大小写不一致

CSV 文件名可能是：

```text
Train.csv
```

但你写的是：

```python
"train.csv"
```

在很多系统里，大小写不同就是不同文件。遇到 `FileNotFoundError` 时，先用：

```python
print(os.listdir(path))
```

确认真实文件名。

### 3. 忘记导入 pandas

错误示例：

```python
train = pd.read_csv("train.csv")
```

如果前面没有：

```python
import pandas as pd
```

就会报：

```text
NameError: name 'pd' is not defined
```

### 4. 当前工作目录不对

有些教程会直接写：

```python
train = pd.read_csv("train.csv")
```

这要求当前运行目录下刚好有 `train.csv`。在 Kaggle 或本地项目里，当前目录不一定是数据所在目录，所以更推荐写清楚完整路径：

```python
train = pd.read_csv(os.path.join(path, "train.csv"))
```

## 我自己的记忆方式

可以把这行代码读成一句话：

```python
train = pd.read_csv(os.path.join(path, "train.csv"))
```

意思是：

```text
把 path 文件夹下面的 train.csv 读进来，存到 train 这个表里。
```

再把：

```python
print(len(train))
```

读成：

```text
看看训练集一共有多少行样本。
```

## 最小模板

以后每次打开一个 Kaggle 数据集，可以先复制这个模板：

```python
import os
import pandas as pd

path = "/kaggle/input/替换成你的数据集目录"

print(os.listdir(path))

train = pd.read_csv(os.path.join(path, "train.csv"))

print("rows:", len(train))
print("shape:", train.shape)
display(train.head())
```

先把数据读对，再谈特征工程和模型。Kaggle 的第一步不是炫技，而是确认数据真的被你稳稳拿到了。
