
# Detecting AI-Generated Vietnamese Text 🇻🇳🤖

> **A Research & Development Project for Identifying AI-Generated Vietnamese Texts on Social Media.**

## 📖 About The Project
The rapid growth of Large Language Models (LLMs) like GPT and Gemini has significantly improved the quality of AI-Generated Texts (AIGTs), making them increasingly difficult to distinguish from Human-Written Texts (HWTs). While these models are highly capable, AIGTs on social media often lack emotional depth and can be intentionally employed to manipulate information or spread false news. 

To address this issue, we developed a classification model to detect AI-generated texts in the Vietnamese cyberspace. The ultimate goal is to improve the quality of interactions on social media platforms and the web, helping users contribute to the creation of comprehensive and authentic knowledge.

## 📊 Dataset: VSM-D (Vietnamese Social Media Dataset)
Due to the scarcity of high-quality resources for detecting AI-generated text in Vietnamese, we constructed a novel dataset named **VSM-D**.

*   **Data Sources:** The data was gathered from two primary platforms representing different linguistic styles: **Noron.vn** (a Q&A social network with an informal, conversational tone) and **Wikipedia** (formal, academic encyclopedic entries).
*   **Human-Written Data:** To ensure absolute authenticity and eliminate AI contamination, we strictly collected texts published **prior to 2021** (before the widespread adoption of modern LLMs). After a rigorous filtering process, the curated human dataset comprises **4,745 samples**.
*   **AI-Generated Data:** We used state-of-the-art LLMs (ChatGPT, Gemini, Gemma-3) to create corresponding AI texts using two strategies:
    *   *Task 1 (Content Generation):* Prompting the AI to generate entirely new content from scratch based on human topics.
    *   *Task 2 (Content Polishing):* Instructing the AI to rewrite or "polish" existing human texts while retaining the original meaning, simulating an editing assistant.
*   **Prompt Engineering:** We utilized concise **Zero-shot prompts** for Wikipedia and elaborate **Role-playing prompts** for Noron.vn to accurately capture the informal nuances and slang of social media.
*   **Data Split:** The combined dataset was partitioned into training, validation, and testing sets using an **8:1:1 ratio**.

## ⚙️ Methodology
### 1. Data Pre-processing
To optimize model performance and mitigate learning bias, the data underwent a rigorous pre-processing pipeline:
*   **Noise Reduction:** Systematic removal of emojis, markdown syntax, and excessive whitespace.
*   **Stop Words Removal:** Eliminating common Vietnamese words that lack significant semantic meaning.
*   **De-biasing (Mitigation of AI Artifacts):** Using heuristic filters to strip explicit structural markers or standard conversational phrases typical of AI (e.g., "Với tư cách là một trợ lí ảo") so the model learns actual writing styles rather than memorizing keywords.
*   **Length Filtering:** Excluding samples with fewer than **150 characters**, as extremely short texts lack sufficient semantic density and context for accurate detection.

### 2. Investigated Models
We formulated the task as a binary classification problem and fine-tuned several state-of-the-art pre-trained language models:
*   **PhoBERT-base:** The standard RoBERTa-based baseline for Vietnamese NLU tasks.
*   **CafeBERT:** An XLM-RoBERTa based model with extensive Vietnamese-specific pre-training.
*   **ViT5-base & BARTpho-base:** Advanced Text-to-Text and Sequence-to-Sequence architectures optimized for generative and classification tasks.
*   **mDeBERTa-v3-base:** A robust multilingual model from Microsoft used as a baseline comparison.

## 🏆 Experimental Results
**Vietnamese-centric models significantly outperformed the multilingual baseline**, proving that language-specific pre-training is crucial for capturing linguistic nuances. Below are the highlights on the Combined Test Set (mixed of Generated and Polished AI texts):

| Model | Accuracy (%) | Precision (%) | Recall (%) | F1-score (%) |
| :--- | :---: | :---: | :---: | :---: |
| PhoBERT | 97.27 | 97.17 | 97.68 | 96.92 |
| **CafeBERT** | **98.70** | **98.72** | **98.37** | **98.54** |
| ViT5 | 97.68 | 97.63 | 97.14 | 97.38 |
| BARTpho | 97.34 | 97.03 | 96.98 | 97.00 |
| mDeBERTa | 91.55 | 89.90 | 91.92 | 90.74 |

*(Data derived from Table 2 of the report)*.

**Key Finding:** The **"AI Polished" task proved to be significantly more challenging** than the "AI Generated" task across all models. When AI is used to edit human text, it retains a large portion of the original human stylistic features, making it much harder for classifiers to distinguish.

## 💻 Demo Web Application
To demonstrate practical applicability, we developed a real-time web application powered by our best-performing model, **CafeBERT**.
*   **Interface:** Built with React, featuring a minimalist design where users can input Vietnamese text segments.
*   **Output:** Returns classification results (Human-written, AI-generated, or AI-polished) along with prediction probabilities.
*   **Performance:** The inference latency is highly optimized, averaging only **200ms per request** in an RTX 5070 GPU environment.

## 🚧 Challenges and Limitations
*   **Evolving Generative AI:** The continuous improvement of LLMs creates an "arms race", diminishing the statistical gap between human and machine text and making detection increasingly difficult.
*   **Linguistic Complexity:** Social media texts heavily feature slang, abbreviations, and **code-switching** (mixing English and Vietnamese), which adds noise and can cause false positives. 
*   **Generalizability & Scalability:** The current dataset is restricted to social media and encyclopedic contexts; the model's performance on unseen genres (e.g., creative literature, legal documents) remains unverified.
*   **Lack of Stress-Testing:** The system has not yet been stress-tested against emerging proprietary architectures or newer Vietnamese-centric LLMs.
*   **Black-Box Nature:** The detector lacks explainability; it cannot output span-level explanations detailing *why* a specific text is classified as AI.

## 👨‍🔬 Authors
This project was conducted by researchers from the **University of Science, Vietnam National University – Ho Chi Minh City**:
*   **T. M. Duyen Ngo**
*   **H. U. Thu Le**
*   **L. L. Phuc Nguyen**
*   **Van-Hoang Phan**
