# 🤟 OmniGesture — AI Sign Language Recognition using Vision Transformer (ViT)

> **⚠️ Work in Progress** — The model is currently achieving ~45_55% accuracy on 100 ASL classes. Actively working on improving it!

---

## 📌 What is OmniGesture?

OmniGesture is a deep learning project that recognizes **American Sign Language (ASL)** gestures from short video clips. You show it a hand sign — it tells you the word.

It is built using a **Vision Transformer (ViViT)** — a modern AI model that understands motion in video by breaking it into small chunks and studying patterns across time
and space.

---

## 🧠 How It Works (Simple Explanation)

```
📹 Video Clip (hand sign)
        ↓
🧩 Split into 16-frame chunks (Tubelet Embedding)
        ↓
🔍 Vision Transformer studies motion patterns
        ↓
🏷️ Outputs the ASL word label
```

---

## 📂 Project Structure

```
OmniGesture/
│
├── data/
│   ├── raw/                  # Downloaded WLASL videos + manifest
│   └── processed/            # Preprocessed .npz clip shards
│
├── checkpoints/              # Saved model weights
├── logs/                     # Training loss & accuracy curves
├── src/                      # Python modules
│
└── OmniGesture_ViT.ipynb     # Main notebook (all phases)
```

---

## 🗂️ Dataset

- **WLASL (World-Level American Sign Language)**
- Source: [github.com/dxli94/WLASL](https://github.com/dxli94/WLASL)
- This project uses the **top 100 most common ASL words** from the dataset
- Videos are downloaded via `yt-dlp` and preprocessed into 16-frame clips of size `224×224`

---

## ⚙️ Tech Stack

| Tool | Purpose |
|------|---------|
| PyTorch | Deep learning framework |
| HuggingFace Transformers | Pretrained ViViT model |
| OpenCV | Video decoding & preprocessing |
| MediaPipe | Hand landmark detection (demo) |
| Gradio | Live web demo interface |
| einops | Tensor reshaping for tubelet embeddings |
| Google Colab (T4 GPU) | Training environment |

---

## 🏋️ Training Strategy

Training is done in **2 stages** to avoid destroying pretrained knowledge:

### Stage 1 — Head Only (5 epochs)
- Freeze the entire ViViT backbone
- Only train the final classification layer
- Learning rate: `1e-3`
- Expected accuracy: **~40–50%**

### Stage 2 — Fine-Tuning (10 epochs)
- Unfreeze top 2 attention blocks + normalization layer
- Train with very low learning rate: `1e-5`
- Expected accuracy: **~65–75%**

> This two-stage approach prevents **catastrophic forgetting** — the model keeps its pretrained video understanding while learning ASL-specific patterns.

---

## 📊 Results

| Stage | Epochs | Accuracy |
|-------|--------|----------|
| Stage 1 (Head only) | 5 | ~40–50% |
| Stage 2 (Fine-tuned) | 10 | ~65–75% |

> ⚠️ **Known Issue:** Accuracy still has room for improvement. Currently working on: better data augmentation, longer training, and exploring larger ViT variants.

---

## 🚀 How to Run

### 1. Open in Google Colab
Upload `OmniGesture_ViT.ipynb` to Google Colab and set runtime to **T4 GPU**.

### 2. Install Dependencies
```bash
pip install torch torchvision torchaudio
pip install einops transformers accelerate datasets
pip install opencv-python-headless tqdm
pip install mediapipe gradio yt-dlp
```

### 3. Mount Google Drive
```python
from google.colab import drive
drive.mount('/content/drive')
```

### 4. Run All Phases in Order
- **Phase 1:** Data download & preprocessing
- **Phase 2:** Model architecture (ViViT)
- **Phase 3:** Training (Stage 1 + Stage 2)
- **Phase 4:** Evaluation & confusion matrix
- **Phase 5:** Gradio live demo

---

## 🎯 Roadmap / What's Next

- [ ] Fix accuracy issues with better augmentation
- [ ] Train on more than 100 classes (WLASL-300, WLASL-1000)
- [ ] Add real-time webcam support via MediaPipe
- [ ] Export model to ONNX for deployment
- [ ] Build a mobile-friendly Gradio interface

---

## 🙋 Why I Built This

Millions of people around the world use sign language every day. A reliable AI sign language recognizer could one day help bridge the 
communication gap between the hearing and deaf communities.

This is my step 1. It's not perfect yet — but it's real, it works, and I'm improving it every day. 💪

---

## 📄 License

MIT License — free to use, modify, and share.

---

## 🤝 Contributing

Pull requests are welcome! If you find a bug or have an idea to improve accuracy, feel free to open an issue.

---

*Built with ❤️ using PyTorch + HuggingFace Transformers*
