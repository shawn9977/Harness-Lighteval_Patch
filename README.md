
# Harness & Lighteval Patch Guide

This guide provides step-by-step instructions to apply custom patches to the `lm-evaluation-harness` and `lighteval` tools, enabling model evaluation on Intel XPUs.

---

## 1. Harness Patch

### ✅ Clone and Checkout `lm-evaluation-harness`

```bash
git clone https://github.com/EleutherAI/lm-evaluation-harness
cd lm-evaluation-harness
git checkout 091aaf6f31f3e87e81135a5d204c30707711d94a
```

### 🔧 Apply the Patch

```bash
curl -fSL -O https://raw.githubusercontent.com/shawn9977/Harness-Lighteval_Patch/main/0001-My-custom-modifications-for-patch.patch
git apply --check 0001-My-custom-modifications-for-patch.patch
git apply 0001-My-custom-modifications-for-patch.patch
```

### 📌 Patch Highlights

1. **Custom LLM Engine Import**
   - Replaces the default `vllm.LLM` with `IPEXLLMClass` from `ipex_llm.vllm.xpu.engine` to support Intel XPUs.
   - Retains `SamplingParams` from VLLM for sampling consistency.

2. **Device Argument Update**
   - Changes the default device argument from `"cuda"` to `"xpu"` to align with Intel's heterogeneous backend.

---

## 2. Lighteval Patch

### ✅ Clone and Checkout `lighteval`

```bash
git clone https://github.com/huggingface/lighteval.git
cd lighteval
git checkout d805f9fa0a84da9ca4c0c6a638bbed149a7012a3
```

### 🔧 Apply the Patch

```bash
curl -fSL -O https://raw.githubusercontent.com/shawn9977/Harness-Lighteval_Patch/main/0002-My-custom-modifications-for-patch.patch
git apply --check 0002-My-custom-modifications-for-patch.patch
git apply 0002-My-custom-modifications-for-patch.patch
```

### 📌 Patch Highlights

1. **YAML Configuration Changes (`vllm_model_config.yaml`)**
   - Replaces HuggingFace model with a local DeepSeek-R1 path.
   - Sets the `revision` to `main`.
   - Adds Intel-specific options:
     - `distributed_executor_backend: ray`
     - `load_in_low_bit: fp8`
   - Optimizes performance parameters:
     - `gpu_memory_utilization`: from 0.6 → 0.8
     - `max_model_length`: set to 35587
     - `swap_space`: from 4 → 16
     - Increases `max_num_batched_tokens` and `max_new_tokens`

2. **Python Model Wrapper Changes (`vllm_model.py`)**
   - Replaces `vllm.LLM` with `IPEXLLMClass`.
   - Adds new fields to `VLLMModelConfig`:
     - `distributed_executor_backend`
     - `load_in_low_bit`
   - Adds new engine arguments during model initialization:
     - `load_in_low_bit`, `disable_async_output_proc`, `enforce_eager`, `block_size`
   - Ensures compatibility with IPEX-LLM quantized backend.

---
