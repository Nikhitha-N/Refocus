# 🪞 REFocus: Real-world Motion Blur Recovery & Optional Colorization

**REFocus** is a two-stage AI pipeline that restores blurred or defocused real-world images and optionally colorizes grayscale inputs.  
It is designed for real photos from smartphones, CCTV, or scanned archives — producing clear, natural, and shareable results.  

---

## 🧩 Overview

Modern cameras often capture **motion-blurred** or **low-light** scenes due to handshake, fast motion, or defocus.  
While “auto enhance” filters tend to oversharpen, **single-stage neural networks** often fail on realistic camera blur.  

REFocus solves this with a **luminance-first conditional colorization pipeline**:

1. **Stage-1 (Restoration)** – Deblur and denoise the image using **DeblurGAN** on the **luminance (L)** channel only.  
2. **Stage-2 (Colorization)** – Apply **CycleGAN** colorization **only** when the image is grayscale, preserving authentic color when possible.

---

## 🎯 Project Goals
- Recover structure and clarity from motion-blurred or defocused photos.  
- Preserve original hue and luminance while optionally adding realistic color to grayscale inputs.  
- Deliver an intuitive, drag-and-drop **Streamlit web app** with side-by-side “Before/After” previews.

---
## 🔎 Repository Layout
```text
.
├─ Notebooks/
│  ├─ image-restoration.ipynb      # DeblurGAN (L-channel) training/eval
│  └─ Colorization.ipynb            # U-Net colorization (L → ab) training/eval
├─ UI/
│  ├─ Interface.ipynb                    # Streamlit app
├─ models/                          # Saved weights/checkpoints
├─ Results/                         # Curves, sample outputs, panels
├─ Reports/                         # IEEE-format deliverables
├─ requirements.txt
└─ README.md
```
---

## 📦 Datasets

### Restoration — HIDE (Human-Aware Motion Deblurring), Kaggle
Paired blurred ↔ sharp street/person scenes. Used to train Stage-1 on the brightness image (grayscale).

Train/Val: ~90/10 from HIDE train; Test: HIDE test

Patches: 256×256 with paired transforms (same crop/flip for both images)

you can the dataset from the link https://www.kaggle.com/datasets/darthvader4067/hideblur

### Colorization — MS-COCO 2017 (via Hugging Face metadata)
Large, diverse natural images to learn color priors. Used to train Stage-2 to add colors when the input is grayscale.

Train: 95% of COCO-train; Val: 5% of COCO-train; Test: COCO-val (5k)

Loaded by URL with on-disk cache in Colab (or you can download zips once)
you can access the dataset from following link https://huggingface.co/datasets/nickpai/coco2017-colorization/viewer?views%5B%5D=train


---

## 🚀 Running Locally

```bash
# Clone this repository
git clone https://github.com/<your-username>/refocus.git
cd refocus

# Open jupyter notebook
jupyter notebook
 ```
---

## **Summary table**

| Component         | Dataset / Split            | Key Metric(s)                                   | Notes                          |
|------------------|----------------------------|--------------------------------------------------|--------------------------------|
| DeblurGAN-v2 (L) | HIDE (90/10 + test)        | Test content-loss ≈ **0.114**                   | Stable after **50 epochs**     |
| U-Net Colorizer  | COCO (95/5 + val2017 test) | **MSE 0.0054**, **PSNR 24.32 dB**, **SSIM 0.9180** | Train–val gap ~epoch **25–30** |

---
## ⚠️ Known Issues

- Large images require tiling; seam-aware overlap-add planned.
- Colorization can overfit after ~25–30 epochs; needs stronger regularization and broader palette diversity.
- Rare palettes may appear muted; identity/cycle tuning helps if using the CycleGAN variant.
- UI: progress indicator and batch mode are planned.

---
## 📝 Notes: Train vs. Use Pretrained

**Fast start (recommended): use pretrained weights**
1. Download/place the checkpoints in `models/`:
   - `models/deblurgan_v2_L_best.pt`   # Stage-1 (luminance deblurring)
   - `models/unet_colorizer_best.pt`   # Stage-2 (L→ab colorization)
2. Launch the UI (or notebook) and run inference directly:
   - Interface.ipynb: It is the file for UI.

**Train (or fine-tune) the models**
1. Prepare datasets (HIDE for deblurring, COCO for colorization) per the README.
2. Open the training notebooks and run all cells:
   - Stage-1: `Notebooks/image-restoration.ipynb` (DeblurGAN on L channel)
   - Stage-2: `Notebooks/Colorization.ipynb` (U-Net L→ab)
3. New checkpoints will be saved under `models/`. Update the UI/notebooks to point to your new `.pt` files.

> Tip: If you only need deblurring, you can skip Stage-2 entirely. If your input is grayscale, enable Stage-2 for colorization.

---

## 🚀 Author Information
Nikhitha Nagalla|

nikhithanagalla@ufl.edu|

Applied Data Science|

University of Florida
