***AI INTERNSHIP PROJECT REPORT:***  
***Automated Question Quality & Bloom’s Taxonomy Checker***  
*Made by: Maira K.B. Wala*

---

# **APPENDIX A: STUDENT AI SCOPING CANVAS**

**Problem Statement Selection**  
 Automated Question Quality & Bloom’s Taxonomy Checker (Problem \#9) from the internship handbook.

**User Persona**  
Primary user is a senior school student (Grades XI–XII) who creates practice questions for peer learning, competitive exams, or self-study.

**Data Strategy**  
 The vision component uses a binary image classifier trained to distinguish between *Printed Questions* (textbook/test papers) and *Handwritten Questions* (notebook/rough work).  
 Estimated data: \~20 images per class (Printed vs Handwritten).  
 No sensitive biometric data (faces/voices) is collected.

**The “Intelligence” Goal**  
 The Gemini API will analyze the typed question, classify it according to Bloom’s 6-level taxonomy (Remember, Understand, Apply, Analyze, Evaluate, Create), and provide a “Pro-Tip” for elevating the question to a higher cognitive level.  
 This reasoning layer adds pedagogical value beyond a simple classifier.

**Success Metric**  
 System must correctly tag “What is…” questions as low-order (e.g., Remember) and “How would you…” or “Why…” questions as higher order (e.g., Apply/Analyze/Evaluate).  
 The interface should provide a clear suggestion for improving the question’s cognitive level.

**End-User Scenario**  
 Students type questions into a Streamlit UI, optionally capture an image of their handwritten/printed question, and receive Bloom-Level feedback to improve academic rigor.

**Technical Context & Environment**  
 Runs on a laptop with webcam for basic perception layer. Users interact through a browser interface in Streamlit.

# **APPENDIX B: AI ETHICS & BIAS AUDIT**

This audit was conducted during Hour 9 of the project as part of mandatory documentation to evaluate the limitations, fairness, and ethical compliance of the AI model developed.

## **1\. Failure Mode Analysis**

AI systems are not perfectly accurate and can fail under certain conditions. During testing, the following failure scenarios were observed:

### **Scenario 1 – Environmental Failure**

During testing, the model’s performance reduced significantly under poor environmental conditions.

* When the camera was tilted at an angle and lighting was dim, the model failed to correctly identify the Image   
* Shadows and background clutter caused the AI to confuse the printed Image as handwritten  
* In some trials, glare from overhead light altered the appearance of the page, leading to incorrect classification.

**Reason:**  
 The model was primarily trained on images captured in well-lit, straight-angle conditions and uploaded images from the internet. It struggled when real-world conditions varied from the training environment.

### **Scenario 2 – Data Failure**

The AI misclassified an object that was not well represented in the training dataset.

* A **dirty/crushed/partially covered Image** was incorrectly classified.  
* Similarly, partially hidden Images were sometimes confused with other categories.

**Reason:**  
 The training data lacked sufficient examples of edge cases such as damaged, dirty, rotated, or partially visible Images.

## **2\. Bias Mitigation Strategy**

Steps were taken to ensure the AI system is fair, inclusive, and capable of handling real-world variation.

### **Diversity in Data**

To reduce bias, the training dataset included:

* Images captured under different lighting conditions (bright, dim, natural light).

* Papers placed against different backgrounds.

* Images/papers placed at different angles and distances from the camera.

* Variations such as clean, dirty, rotated, and partially covered sheets or images.

This helped the AI learn from diverse visual patterns instead of ideal conditions only.

**Edge Case Testing**

The AI was deliberately tested with borderline examples to check robustness:

* Testing when the Notebook was partially out of frame.  
* Testing when multiple papers were present in the background.  
* Testing from unusual angles to observe model behavior.

These tests helped identify weaknesses and improve understanding of the model’s limits.

## **3\. Data Privacy & Ethical Compliance**

Ethical handling of data was strictly followed during this project.

### **Consent Log**

No person’s face or voice was used without permission.  
If any human data was used during testing, prior verbal consent was obtained.  
Most of the training data consisted of objects, not personal identity data.

### **Storage Transparency**

No sensitive personal data (such as facial data or voice recordings) was permanently stored.  
All images used for training were stored locally and only for project purposes.  
Data was not shared online or with any third party.  
After training and testing, unnecessary data was deleted to maintain privacy compliance.

