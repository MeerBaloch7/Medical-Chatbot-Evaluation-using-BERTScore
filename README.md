# 🧠 Medical Chatbot Evaluation using BERTScore

This project demonstrates an evaluation framework for assessing the quality of doctor-patient dialogues in a medical chatbot using **BERTScore**, especially tailored for medical question relevance.

---

## 📌 Overview

In a simulated medical training environment, a chatbot plays the role of a patient, while a medical student or professional asks diagnostic questions. The chatbot responds based on a fixed medical context. The goal of this project is to evaluate **how relevant and appropriate** the doctor’s questions are, by comparing them with a set of pre-defined expert (ground truth) questions.

---

## 🧪 What is BERTScore?

[BERTScore](https://arxiv.org/abs/1904.09675) is an evaluation metric for natural language generation tasks that:

- Computes token-level similarity using contextual embeddings from BERT or other transformer models.
- Measures **Precision**, **Recall**, and **F1 Score** between candidate and reference texts.
- Goes beyond surface-level matching like **BLEU** or **ROUGE** by incorporating contextual and semantic similarity.

### 🔍 Comparison with Traditional Metrics

| Metric     | Token-based | Context-aware | Synonym-aware | Suitable for Long Texts |
|------------|-------------|---------------|----------------|--------------------------|
| BLEU       | ✅ Yes       | ❌ No          | ❌ No           | ❌ Poor                  |
| ROUGE      | ✅ Yes       | ❌ No          | ❌ No           | ✅ Somewhat              |
| **BERTScore** | ❌ No        | ✅ Yes         | ✅ Yes          | ✅ Yes                   |

---

## 🧬 BERT Model Used

We use [`emilyalsentzer/Bio_ClinicalBERT`](https://huggingface.co/emilyalsentzer/Bio_ClinicalBERT), a BERT-based model pretrained on clinical notes from the MIMIC-III dataset. It’s well-suited for understanding medical terminology and clinical context, making it an excellent choice for evaluating medical dialogues.

---

## 🤖 How It Works

1. **Chatbot Simulation**:
   - A medical chatbot (LLama 3.1 Aloe Beta 8B) plays the role of a patient.
   - A human user (doctor/student) asks diagnostic questions in a dialogue.
   - The chatbot responds based on a fixed medical context.

2. **Dialogue Collection**:
   - Each user-patient interaction is stored (doctor’s question and chatbot’s response).

3. **Ground Truth**:
   - A JSON file contains the reference (expert-written) doctor dialogue for the same patient case.

4. **Alignment & Matching**:
   - The system aligns user questions to ground truth questions using similarity matching.
   - Top relevant question pairs are selected for scoring.

5. **Evaluation using BERTScore**:
   - Each doctor question is scored against the best matching ground truth question using BERTScore (Precision, Recall, F1).
   - A summary report is generated including:
     - Per-question scores
     - Overall averages
     - Alignment accuracy (e.g., “8/10 correct, 80% aligned”)

---

## 📊 Output Example

```json
{
  "results": [
    {
      "student_question": "What do you do for work?",
      "ground_truth_question": "what kind of work do you do",
      "patient_response": "I'm a landscaper.",
      "bertscore_precision": 0.88,
      "bertscore_recall": 0.91,
      "bertscore_f1": 0.89
    },
    ...
  ],
  "summary": {
    "average_precision": 0.87,
    "average_recall": 0.89,
    "average_f1": 0.88,
    "alignment_score": "8/10 matched (80%)",
    "total_questions": 10
  }
}
