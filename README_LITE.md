# EAS_nanoowl（精簡版）

## 0. 專案介紹

### 0.1 介紹
EAS_nanoowl 提供以文字驅動（prompt-driven）的視覺推論能力，可在邊緣裝置上執行即時目標偵測與樹狀語意推論（Tree Prediction），適合智慧製造、零售場域、公共空間與工業監控等情境的快速 PoC 與量產導入。

- **Category**：通用領域（General-purpose Edge Vision AI）

<p align="center">
  <img src="https://raw.githubusercontent.com/NVIDIA-AI-IOT/nanoowl/main/assets/jetson_person_2x.gif" width="70%" />
</p>

### 0.2 模型介紹
EAS_nanoowl 核心採用 NanoOWL 技術路線，整合以下模型能力：

- **OWL-ViT（Open-Vocabulary Detection）**  
  以自然語言描述目標類別，支援不需重訓即可進行開放詞彙偵測。
- **Tree Prompt 推論流程**  
  支援巢狀提示（nested prompts），可在同一流程中完成「物件偵測 + 細部屬性/子物件判定」。
- **TensorRT 加速路徑**  
  使用 TensorRT engine 提升推論效能，兼顧即時性與部署可行性。

### 0.3 支援平台

| Platform | Hardware Spec | OS / SDK | 連結 |
|---|---|---|---|
| [AIR-075](https://docs.edge-ai-sdk.advantech.com/docs/Hardware/AI_System/Nvidia/Jetson%20Thor/AIR-075) | RAM: 128/64 GB, Storage: 512 GB | JetPack 7.1 | [安裝](https://docs.edge-ai-sdk.advantech.com/docs/Turtorial/Document/Linux/User_Manual-3.0) |

---

## 1. 環境建置

### 1.1 AI-Stack Check Info
請先確認目標裝置的 AI Stack 狀態：

```bash
cat /etc/nv_tegra_release
nvcc --version
python3 --version
docker --version
```

### 1.2 需要可存取相機（例如 `/dev/video0`）

```bash
ls -l /dev/video*
```

### 1.3 其他必要條件
- 可連外下載模型

---

## 2. 開發與佈署

### 2.1 使用 Docker YAML 啟動

```bash
MODEL_DIR=/opt/Advantech/EdgeAI/System/Nvidia_Jetson/VisionAI/app/nanoowl
mkdir -p "$MODEL_DIR"
MODEL_DIR="$MODEL_DIR" docker compose up -d
MODEL_DIR="$MODEL_DIR" docker compose logs -f eas_nanoowl
```

### 2.2 服務驗證

```bash
curl -I http://127.0.0.1:7860/
```

UI 入口：`http://<device-ip>:7860`

### 2.3 使用 Docker YAML 關閉

```bash
MODEL_DIR=/opt/Advantech/EdgeAI/System/Nvidia_Jetson/VisionAI/app/nanoowl
MODEL_DIR="$MODEL_DIR" docker compose down
```
