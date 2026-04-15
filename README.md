# hf-mirror_fastdownload
对hf-mirror网址提供的hfd.sh加速下载脚本的优化使用批处理脚本。

```
#!/bin/bash
set -e  # 遇错立即终止

# 1. 激活 Conda 环境
source ~/miniforge3/bin/activate hfd

# 2. 输入基础保存目录
read -p "📂 请输入模型权重基础保存目录: " BASE_DIR
if [ -z "$BASE_DIR" ]; then
    echo "❌ 路径不能为空！"
    exit 1
fi

# 3. 输入模型 ID
read -p "👉 请输入 Hugging Face 模型权重ID: " MODEL_ID
if [ -z "$MODEL_ID" ]; then
    echo "❌ 模型ID不能为空！"
    exit 1
fi

# 4. 智能生成最终下载路径（自动拼接模型仓库名，严格对齐 hfd.sh 默认行为）
# 提取 org/repo 中的 repo 部分；若 ID 无斜杠则直接使用完整 ID 作为目录名
REPO_NAME="${MODEL_ID#*/}"
FINAL_DIR="$BASE_DIR/$REPO_NAME"

echo "✅ 最终下载路径将自动创建为: $FINAL_DIR"

# 5. 调用 hfd.sh（传入构造好的完整路径）
~/hfd.sh "$MODEL_ID" --local-dir "$FINAL_DIR"

```
