# Qwen2.5-VL-7B LoRA 微调实战

基于 Qwen2.5-VL-7B-Instruct 模型，使用 LoRA 技术在 COCO 2014 Caption 数据集上进行多模态指令微调，实现图像描述生成。

## 项目结构
- `data2csv.py` / `csv2json.py`：数据格式转换
- `train.py`：LoRA 微调训练脚本
- `test.py`：测试集推理脚本
- `coco-2024-dataset.csv`：处理后的训练数据

## 技术方案
- **基础模型**：Qwen2.5-VL-7B-Instruct
- **微调方法**：LoRA（r=64, alpha=16, dropout=0.05）
- **训练配置**：3 epoch，等效 batch size 16，学习率 1e-4
- **实验记录**：[SwanLab 训练曲线](你的 SwanLab 链接)

## 训练结果
- Loss 从 4.5 平稳下降并收敛至 1.6 左右
- 总步数 183 步，耗时约 56 分钟（A100）

## 如何运行
1. 安装依赖：`pip install -r requirements.txt`
2. 数据转换：`python data2csv.py`
3. 训练：`python train.py`
4. 测试：`python test.py`
