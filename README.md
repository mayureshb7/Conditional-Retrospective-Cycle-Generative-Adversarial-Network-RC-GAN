# Conditional Retrospective Cycle Generative Adversarial Network (RC-GAN)

**Authors**:  
[Mayuresh Bhosale](https://www.linkedin.com/in/mayuresh-bhosale-b6b935136/)
[Prakhar Gupta](https://www.linkedin.com/in/prakharkalyangupta/)


**Institution**: Clemson University  

**Course**: CPSC 8810 – Machine Learning for Image Synthesis (Dr. Siyu Huang)

**Project Assets**:  
- 📄 [Final Report](/assets/files/report.pdf)  
- 📊 [Presentation Slides](/assets/files/cond_rcgan.pptx)

---

## Motivation & Key Idea

This project builds upon **Retrospective Cycle GAN** proposed by Kwon et al. (CVPR 2019). Their work demonstrated strong performance compared to state-of-the-art methods using forward and backward temporal consistency for future frame prediction.  

However, their approach **does not explicitly condition on physics** or restrict pixel motion, which leads to noticeable blurring and distortions in long-term predictions.

<p align="center">
  <img src="assets/images/kitti_paper.png" width="450"/>
  <br/>
  <em>
    Figure 1: Blurring and distortions in long-term predictions observed in Retrospective Cycle GAN (Kwon et al., 2019).
  </em>
</p>

We ask the following question:

> **Can blurring in long-term future predictions be reduced by incorporating physics-based constraints?**

---

## Model Formulation

### RCGAN Baseline Model

The baseline Retrospective Cycle GAN uses **two discriminators**:
1. Image reconstruction discriminator
2. Temporal consistency discriminator

The overall loss function is shown below:

<p align="center">
  <img src="assets/images/rcgan_loss.png" width="350"/>
</p>

---

### Optical-Flow Conditioned RCGAN

To indirectly restrict pixel motion to physically plausible regions, we condition the GAN using **optical flow loss** obtained from the pretrained **RAFT** model (Teed et al.).

<p align="center">
  <img src="assets/images/raft_flow.png" width="550"/>
  <br/>
  <em>
    Figure 2: Optical flow estimation performance of RAFT on the KITTI dataset.
  </em>
</p>

The modified loss function becomes:

<p align="center">
  <img src="assets/images/rcgan_flow_loss.png" width="480"/>
</p>

---

### Kinematics-Constraint Conditioned RCGAN

In this approach, we incorporate **vehicle kinematic constraints** (velocity-based motion limits) into the GAN training objective.

<p align="center">
  <img src="assets/images/rcgan_kine_loss1.png" width="580"/>
</p>

---

### Combined Constraints Conditioned RCGAN

Finally, we combine **optical-flow** and **kinematic** constraints into a unified loss formulation.

<p align="center">
  <img src="assets/images/rcgan_combo_loss.png" width="390"/>
</p>

---

## Experimental Results

The models were trained on **KITTI city driving sequences** of length 5 and evaluated on approximately **70 test sequences**.

Results below compare the **baseline**, **optical-flow conditioned**, and **kinematics-conditioned** models at different training epochs.

---

### Epoch 10

<p align="center">
  <img src="assets/images/result_0.png"/>
</p>

<p align="center">
  <img src="assets/images/results_e10.png"/>
</p>

<p align="center">
  <img src="assets/images/results_e10_tab.png" width="350"/>
</p>

---

### Epoch 30

<p align="center">
  <img src="assets/images/results_e30.png"/>
</p>

---

### Epoch 50

At epoch 50, the **kinematics-conditioned RCGAN** already shows reduced blurring compared to the baseline and optical-flow conditioned models.

<p align="center">
  <img src="assets/images/results_e50.png"/>
</p>

---

### Epoch 100

At epoch 100, the kinematics-conditioned model clearly outperforms all other approaches.  
The combined conditioning model shows promising **PSNR, SSIM, and MSE** metrics, though it requires additional training due to the increased loss complexity.

<p align="center">
  <img src="assets/images/results_e99.png"/>
</p>

<p align="center">
  <img src="assets/images/results_e99_combined.png"/>
</p>

<p align="center">
  <img src="assets/images/results_e99_tab1.png" width="400"/>
</p>

Additional qualitative results for kinematics-conditioned predictions:

<p align="center">
  <img src="assets/images/results_kineExtra.png"/>
</p>

---

## Project Insights & Conclusions

We propose a **conditional GAN framework** that restricts pixel motion using **optical flow** and **vehicle kinematic constraints**. These physics-inspired conditions significantly reduce long-term blurring compared to the baseline Retrospective Cycle GAN.

This work represents an initial step toward **differential-equation-based physics conditioning** in generative models using object-level pixel tracking.

---

## References

- Kwon et al., *Predicting Future Frames Using Retrospective Cycle GAN*, CVPR 2019  
- Teed & Deng, *RAFT: Recurrent All-Pairs Field Transforms for Optical Flow*
