# 🧠 ANN Surrogate for Aerodynamic Simulation  

## 📌 Overview  
This project demonstrates how a **deep neural network (U-Net)** can act as a **surrogate model** for traditional **Computational Fluid Dynamics (CFD)** simulations.  

- **The Problem**: High-fidelity CFD simulations are computationally expensive, often taking hours/days → impractical for real-time design.  
- **The AI Solution**: A trained U-Net predicts **pressure** and **velocity fields** around new airfoils in **milliseconds**, enabling rapid design optimization.  

---

## ✨ Key Features  
- ⚡ **Instantaneous Prediction** – from hours to milliseconds.  
- 📘 **Physics-Informed Learning** – learns fluid dynamics patterns from simulated datasets.  
- 🖼️ **Image-to-Image Translation** – maps airfoil shape → aerodynamic flow field.  
- 📊 **Scalable Data Generation** – includes scripts to generate thousands of training samples.  

---

## ⚙️ How It Works  
The workflow has **three phases**:  

### 1️⃣ Data Generation  
- Uses a **Python-based panel method solver**.  
- Varies:  
  - **Airfoil Shape** (NACA 4-digit, e.g., 2412, 4415)  
  - **Angle of Attack (AoA)**  
- Each sample =  
  - Input: **128×128 binary mask** (airfoil shape)  
  - Output: **3 fields** (pressure coefficient, x-velocity, y-velocity).  

### 2️⃣ Model Architecture – U-Net  
- **Encoder-Decoder with Skip Connections**  
- Encodes airfoil geometry → latent features.  
- Decodes → full-resolution flow fields.  
- Skip connections preserve fine details (edges, sharp boundaries).  

### 3️⃣ Training & Evaluation  
- Dataset split: **70% train / 15% val / 15% test**.  
- **Loss Function**: Mean Squared Error (MSE).  
- Evaluates predictions vs. ground truth simulations.  

---

## 🚀 Getting Started  

### 🔧 Prerequisites  
Install dependencies:  
```bash
pip install numpy tensorflow scikit-image scikit-learn tqdm matplotlib
