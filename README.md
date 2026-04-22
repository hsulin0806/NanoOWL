# EAS_nanoowl

Open-vocabulary / Tree Prompt vision inference on edge devices, based on NanoOWL.

## 專案資訊

### 1) 專案定位
EAS_nanoowl 提供可落地的邊緣 AI 視覺推論方案，支援以文字 prompt 進行即時目標偵測與樹狀語意辨識（Tree Prediction）。

### 2) 為什麼先用 Edge_AI_SDK InferenceKit
- 參考連結：<https://ess-wiki.advantech.com.tw/view/Edge_AI_SDK/InferenceKit>
- 建議流程：**先安裝 Edge_AI_SDK InferenceKit，再安裝 EAS_nanoowl**
- 主要好處：
  - 由 InferenceKit 先處理 AI 基礎環境（驅動 / runtime / 相依套件）
  - 開發者不必自行手動處理複雜系統環境
  - 降低部署風險，加快 PoC 到量產導入時程

### 3) 已驗證硬體平台
| 平台 | 驗證狀態 | 連結 | 備註 |
|---|---|---|---|
| AIR-075 | ✅ 已驗證可運作 | <https://www.advantech.com/en-us/products/932c8818-07cc-4917-89e9-7a678ddc029c/air-075/mod_8489cdc1-ab25-48e3-a493-085d8db1860f> | JetPack 7.x / CUDA 13 路線 |
| （預留）其他平台 | ⏳ 待驗證 | - | 後續補充硬體與版本資訊 |

### 4) 執行結果示意
<p align="center">
  <img src="https://raw.githubusercontent.com/NVIDIA-AI-IOT/nanoowl/main/assets/jetson_person_2x.gif" width="48%" />
  <img src="https://raw.githubusercontent.com/NVIDIA-AI-IOT/nanoowl/main/assets/tree_predict_out.jpg" width="48%" />
</p>

---

## 開發者資訊（安裝與執行）

> Upstream 參考：<https://github.com/NVIDIA-AI-IOT/nanoowl>

### 0) 前置條件
1. 已完成 Edge_AI_SDK InferenceKit 安裝（建議）
2. 裝置可使用 NVIDIA GPU runtime
3. 可存取 camera（例如 `/dev/video0`）

### 1) 建置容器
```bash
git clone https://github.com/hsulin0806/EAS_nanoowl.git
cd EAS_nanoowl
docker build -t nanoowl:jp7-thor-persist -f docker/jetpack7-thor/Dockerfile .
```

### 2) 準備模型持久化目錄
```bash
MODEL_DIR=/opt/Advantech/EdgeAI/System/Nvidia_Jetson/VisionAI/app/nanoowl
mkdir -p "$MODEL_DIR"
```

### 3) 第一次啟動（線上準備 + 首次轉換）
```bash
docker run --rm --name nanoowl_persist \
  --ipc=host \
  --ulimit memlock=-1 --ulimit stack=67108864 \
  --shm-size=2g \
  --runtime nvidia \
  --device=/dev/video0:/dev/video0 \
  -p 7860:7860 \
  -e MODEL_DIR=/models \
  -e ENGINE_PATH=/models/engines/owl_image_encoder_patch32.engine \
  -v "$MODEL_DIR":/models \
  -v /usr/src/tensorrt:/usr/src/tensorrt:ro \
  -v /usr/lib/aarch64-linux-gnu/libnvinfer_plugin.so.10:/opt/hostlibs/libnvinfer_plugin.so.10:ro \
  -v /usr/lib/aarch64-linux-gnu/libnvinfer_plugin.so.10.13.3:/opt/hostlibs/libnvinfer_plugin.so.10.13.3:ro \
  -e LD_LIBRARY_PATH=/usr/local/lib/python3.12/dist-packages/tensorrt_libs:/opt/hostlibs:/usr/src/tensorrt/lib \
  nanoowl:jp7-thor-persist \
  /opt/nanoowl/docker/jetpack7-thor/run_tree_demo_persistent.sh
```

### 4) 第二次啟動（快取重用）
同一條 `docker run` 指令即可。正常情況下會看到 `engine exists`，且不再進行首次轉換流程。

### 5) 離線啟動（可選）
```bash
docker run --rm --name nanoowl_offline \
  --network none \
  --ipc=host \
  --ulimit memlock=-1 --ulimit stack=67108864 \
  --shm-size=2g \
  --runtime nvidia \
  --device=/dev/video0:/dev/video0 \
  -e MODEL_DIR=/models \
  -e ENGINE_PATH=/models/engines/owl_image_encoder_patch32.engine \
  -v "$MODEL_DIR":/models \
  -v /usr/src/tensorrt:/usr/src/tensorrt:ro \
  -v /usr/lib/aarch64-linux-gnu/libnvinfer_plugin.so.10:/opt/hostlibs/libnvinfer_plugin.so.10:ro \
  -v /usr/lib/aarch64-linux-gnu/libnvinfer_plugin.so.10.13.3:/opt/hostlibs/libnvinfer_plugin.so.10.13.3:ro \
  -e LD_LIBRARY_PATH=/usr/local/lib/python3.12/dist-packages/tensorrt_libs:/opt/hostlibs:/usr/src/tensorrt/lib \
  nanoowl:jp7-thor-persist \
  /opt/nanoowl/docker/jetpack7-thor/run_tree_demo_persistent.sh
```

### 6) Web UI 與快速驗證
- 開啟：`http://<device-ip>:7860`
- 健康檢查：
```bash
curl -I http://127.0.0.1:7860/
```

### 7) 常見問題
- camera 無法開啟：確認 `--device=/dev/video0:/dev/video0`
- 首次啟動較慢：屬正常（模型/engine/warmup）
- 7860 端口衝突：停止舊容器後重啟

---

## 文件規範
後續其他 AI 專案文件，請統一依照：`AI_PROJECT_DOC_STANDARD.md` 撰寫。
可直接複製的 README 範本：`AI_PROJECT_README_TEMPLATE.md`
