# Qwen2.5-VL-7B LoRA 微调实战

基于 Qwen2.5-VL-7B-Instruct 模型，使用 LoRA 技术在 COCO 2014 Caption 数据集上进行多模态指令微调，实现图像描述生成功能。

## 📊 训练实验记录 (SwanLab)
本项目使用了 SwanLab 对训练全过程进行了可视化的监控与记录。
*   **SwanLab 在线实验链接**：[点击此处查看训练图表](https://swanlab.cn/@sxrdfg/Qwen2_5-VL-finetune/v1/82gu9z/runs/3j78p225/chart)

### 核心训练指标 (Loss 收敛数据)
*   **总训练步数**：183 Steps (3 Epochs)
*   **初始 Loss**：约 **4.50**
*   **最终 Loss**：约 **1.65** (模型收敛良好)
*   **训练耗时**：约 **56 分钟** (单卡 A100)
*   **收敛趋势**：前 20 步 Loss 快速大幅下降，随后平稳震荡收敛，未出现梯度爆炸或发散现象。

## 🛠️ 技术方案
*   **基础模型**：`Qwen/Qwen2.5-VL-7B-Instruct`
*   **微调方法**：LoRA (Low-Rank Adaptation)
*   **LoRA 参数**：`r=64`, `lora_alpha=16`, `lora_dropout=0.05`
*   **目标模块**：`q_proj`, `k_proj`, `v_proj`, `o_proj`, `gate_proj`, `up_proj`, `down_proj` (覆盖注意力层与前馈网络层)
*   **训练参数**：`per_device_train_batch_size=4`，`gradient_accumulation_steps=4` (等效 Batch Size 16)，`learning_rate=1e-4`，`num_train_epochs=3`

## 📁 项目结构
*   `train.py`：核心微调训练脚本（含 SwanLab 回调配置）。
*   `test.py`：微调后的模型推理与测试脚本。
*   `data2csv.py` / `csv2json.py`：COCO 数据集格式转换预处理脚本。
*   `coco-2024-dataset.csv`：已处理好的文本与图片路径对应表。

## 🚀 如何运行
1. 环境准备：确保已安装 `transformers`, `peft`, `torch`, `swanlab` 等依赖。
2. 数据准备：下载 COCO 数据集，运行 `python data2csv.py` 和 `python csv2json.py` 生成训练所需的 JSON 文件。
3. 开始训练：运行 `python train.py`。训练过程会自动同步至 SwanLab。
4. 结果测试：修改 `test.py` 中的 checkpoint 路径，运行 `python test.py` 查看模型预测结果。
