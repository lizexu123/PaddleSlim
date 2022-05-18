# 自动压缩工具ACT（Auto Compression Tookit）

## 简介
PaddleSlim推出全新自动压缩工具（ACT），旨在通过Source-Free的方式，自动对预测模型进行压缩，压缩后模型可直接部署应用。ACT自动压缩工具主要特性如下：
- **『更便捷』**：开发者无需了解或修改模型源码，直接使用导出的预测模型进行压缩；
- **『更智能』**：开发者简单配置即可启动压缩，ACT工具会自动优化得到最好预测模型；
- **『更丰富』**：ACT中提供了量化训练、蒸馏、结构化剪枝、非结构化剪枝、多种离线量化方法及超参搜索等等，可任意搭配使用。


## 环境准备

- 安装PaddlePaddle >= 2.2版本 （从[Paddle官网](https://www.paddlepaddle.org.cn/install/quick?docurl=/documentation/docs/zh/install/pip/linux-pip.html)下载安装）
- 安装PaddleSlim >= 2.3 或者适当develop版本

## 快速上手

```python
# 导入依赖包
from paddleslim.auto_compression.config_helpers import load_config
from paddleslim.auto_compression import AutoCompression
from paddleslim.common.imagenet_reader import reader
# 加载配置文件
compress_config, train_config = load_slim_config("./image_classification/mobilenetv1_qat_dis.yaml")
# 定义DataLoader
train_loader = reader(mode='test') # DataLoader
# 开始自动压缩
ac = AutoCompression(
    model_dir="./mobilenetv1_infer",
    model_filename="model.pdmodel",
    params_filename="model.pdiparams",
    save_dir="output",
    strategy_config=compress_config,
    train_config=train_config,
    train_dataloader=train_loader,
    eval_callback=None)  # eval_function to verify accuracy
ac.compress()
```

**提示：**
- DataLoader传入的数据集是待压缩模型所用的数据集，DataLoader继承自`paddle.io.DataLoader`。
- 如无需验证自动压缩过程中模型的精度，`eval_callback`可不传入function，程序会自动根据损失来选择最优模型。
- 自动压缩Config中定义量化、蒸馏、剪枝等压缩算法会合并执行，压缩策略有：量化+蒸馏，剪枝+蒸馏等等。

## 应用示例

#### [图像分类](./image_classification)

#### [目标检测](./detection)

#### [语义分割](./semantic_segmentation)

#### [NLP](./nlp)

#### 即将发布
- [ ] 更多自动压缩应用示例
- [ ] X2Paddle模型自动压缩示例

## 其他

- ACT可以自动处理常见的预测模型，如果有更特殊的改造需求，可以参考[ACT超参配置教程](./hyperparameter_tutorial.md)来进行单独配置压缩策略。

- 如果你发现任何关于ACT自动压缩工具的问题或者是建议, 欢迎通过[GitHub Issues](https://github.com/PaddlePaddle/PaddleSlim/issues)给我们提issues。同时欢迎贡献更多优秀模型，共建开源生态。