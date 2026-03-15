# Forensic_Gunshot_CLIP: Comparing ResNet50 and CLIP in Forensic Pathology

## 🩺 1. Problem Definition & Objectives
Identifying the difference between **Entrance** and **Exit** gunshot wounds is a critical task in forensic pathology for reconstructing shooting incidents. This project explores whether deep learning models can assist pathologists in objectively distinguishing these wounds based on morphology.

The study benchmarks two distinct architectures:
1. **ResNet50 (CNN):** A domain-specific approach focusing on local pathological features (e.g., abrasion rings, skin tearing).
2. **CLIP (Vision Transformer):** A foundation model approach utilizing generalized visual-semantic knowledge.

## 📊 2. Dataset Specifications
The models were evaluated using a specialized dataset of forensic gunshot wound images.
* **Classes:** Entrance Wound vs. Exit Wound
* **Training/Validation/Test Split:** Applied to ensure robust evaluation.
* **Key Challenges:** Class imbalance (Entrance wounds outnumber Exit wounds) and subtle morphological differences.

## 🚀 3. Model Performance & Comparison
| Model | Strategy | Accuracy | Note |
| :--- | :--- | :--- | :--- |
| **ResNet50** | Full Training | **90.1%** | **Best Performer** - Excellent local feature extraction. |
| **CLIP** | Linear Probe | **81.0%** | Good baseline but lacks domain precision. |
| **CLIP** | Fine-tuned | **74.0%** | Divergence observed due to data imbalance and bias. |

## 🛠️ 4. Development & Troubleshooting Log
This repository includes the **Full History** of experiments (`Forensic_Gunshot_CLIP.ipynb`), documenting the following phases:

### Phase 1: CNN Superiority
ResNet50 demonstrated that capturing **local features** is paramount in forensic pathology. It accurately identified the abrasion collar, leading to high diagnostic accuracy.

### Phase 2: CLIP Fine-tuning Challenges
* **Trial & Error:** Initial full fine-tuning led to **Loss Divergence** (Accuracy dropped to 26%). I identified this as 'Catastrophic Forgetting' where the model lost its pre-trained knowledge.
* **Optimization:** Implemented a **Two-Stage Fine-tuning** strategy:
    1. **Warm-up:** Training only the classifier head while freezing the encoder.
    2. **Unfreeze:** Fine-tuning the entire model with an ultra-low learning rate ($1e^{-7}$).
* **Class Weighting:** Applied **Weighted Cross-Entropy** (2.77x weight for Exit wounds) to mitigate class imbalance.

## 🔍 5. Explainable AI (XAI): Aligned Saliency Maps
To validate the model's logic, I implemented **Aligned Attention Maps** to ensure the model focuses on the actual wound site rather than background noise.

![Aligned Attention Map](Aligned%20Attention%20Map.png)

* **Insight:** While ResNet50 focuses on pathological markers, CLIP's attention often shifts toward non-pathological global contexts (e.g., skin texture, hair), explaining its relative underperformance in this niche domain.

## 👨‍⚕️ Author
**Hee Jae Ryu (CowEye)**
* Licensed Physician, South Korea
* 2026 U.S. Pathology Residency Applicant
* Research Interest: Digital Pathology & AI-assisted Forensic Diagnosis
