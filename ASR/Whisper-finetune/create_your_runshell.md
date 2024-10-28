```bash
#!/bin/bash
set -e  # 遇到错误时终止脚本


# parameters
TRAIN_DATA=
TEST_DATA=
OUTPUT=output/
BASE_MODEL=openai/whisper-medium
BATCH_SIZE=16
EPOCHS=5
LEARNING_RATE=2e-4
GRAD_ACC=4
FULL_FINETUNE=False
WARMUP_STEPS=500
USE_4BIT=True

echo "----------------------------------------------------"
echo "TEST_DATA=${TRAIN_DATA}"
echo "TEST_DATA=${TEST_DATA}"
echo "OUTPUT=${OUTPUT}"
echo "BASE_MODEL=${BASE_MODEL}"
echo "BATCH_SIZE=${BATCH_SIZE}"
echo "EPOCHS=${EPOCHS}"
echo "LEARNING_RATE=${LEARNING_RATE}"
echo "GRAD_ACC=${GRAD_ACC}"
echo "FULL_FINETUNE=${FULL_FINETUNE}"
echo "WARMUP_STEPS=${WARMUP_STEPS}"
echo "USE_4BIT=${USE_4BIT}"
echo "----------------------------------------------------"

LOG_DIR="./logs"
mkdir -p "${LOG_DIR}"
exec > >(tee -a "${LOG_DIR}/run.log") 2>&1  # 重定向所有输出到日志文件

export CUDA_VISIBLE_DEVICES=1,2  # 选择显卡

echo "GPU available: ${CUDA_VISIBLE_DEVICES}"
# train
torchrun --nproc_per_node=2 \
    finetune_fix.py \
    --base_model "${BASE_MODEL}" \
    --output_dir "${OUTPUT}" \
    --per_device_train_batch_size "${BATCH_SIZE}" \
    --per_device_eval_batch_size "${BATCH_SIZE}" \
    --num_train_epochs "${EPOCHS}" \
    --learning_rate "${LEARNING_RATE}" \
    --gradient_accumulation_steps "${GRAD_ACC}" \
    --warmup_steps "${WARMUP_STEPS}" \
    --full_finetune "${FULL_FINETUNE}" \
    --use_4bit "${USE_4BIT}" \

echo "----------------------------------------------------"
echo "Training completed!"

```

