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
