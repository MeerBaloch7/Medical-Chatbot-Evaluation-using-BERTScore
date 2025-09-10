# 🧠 Medical Chatbot Evaluation using BERTScore

This project demonstrates an evaluation framework for assessing the quality of doctor-patient dialogues in a medical chatbot using **BERTScore**, specifically designed for medical question relevance and clinical interview assessment.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/MeerBaloch7/Medical-Chatbot-Evaluation-using-BERTScore/blob/main/scoring.ipynb)

---

## 📌 Overview

This project creates a simulated medical training environment where:
- A **LLaMA 3.1 Aloe Beta 8B** model acts as a patient with a specific medical condition
- Medical students or professionals conduct diagnostic interviews by asking questions
- The system evaluates the quality and relevance of doctor's questions using **BERTScore** with clinical BERT embeddings
- Questions are compared against expert-written ground truth dialogues for the same medical case

**Key Innovation**: Uses domain-specific clinical BERT model for more accurate medical dialogue evaluation compared to general-purpose metrics.cal Chatbot Evaluation using BERTScore

This project demonstrates an evaluation framework for assessing the quality of doctor-patient dialogues in a medical chatbot using **BERTScore**, especially tailored for medical question relevance.

---

## 📌 Overview

In a simulated medical training environment, a chatbot plays the role of a patient, while a medical student or professional asks diagnostic questions. The chatbot responds based on a fixed medical context. The goal of this project is to evaluate **how relevant and appropriate** the doctor’s questions are, by comparing them with a set of pre-defined expert (ground truth) questions.

---

## 🚀 Quick Start

### Prerequisites
- Python 3.8+
- CUDA-compatible GPU (recommended)
- Google Colab account (for easy setup)

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/MeerBaloch7/Medical-Chatbot-Evaluation-using-BERTScore.git
   cd Medical-Chatbot-Evaluation-using-BERTScore
   ```

2. **Run in Google Colab (Recommended):**
   - Click the "Open in Colab" badge above
   - Run all cells sequentially
   - The notebook will automatically install all dependencies

3. **Local Installation:**
   ```bash
   pip install transformers bitsandbytes trl accelerate torch bert-score sentence-transformers
   ```

### Usage

1. **Start the interactive chat:**
   ```python
   # The notebook provides an interactive loop where you can:
   # - Ask diagnostic questions as a doctor
   # - Receive patient responses from the AI
   # - Type 'exit' to evaluate your performance
   ```

2. **View evaluation results:**
   - Results are automatically saved to `chat_evaluation.json`
   - Summary includes precision, recall, F1 scores, and alignment metrics

---

## 🛠️ Technical Architecture

### Core Components

1. **Patient Simulation Engine**
   - **Model**: LLaMA 3.1 Aloe Beta 8B
   - **Role**: Simulates patient responses based on medical context
   - **Context**: Pre-defined medical case (e.g., carpal tunnel syndrome)

2. **Question Alignment System**
   - **Model**: `sentence-transformers/all-MiniLM-L6-v2`
   - **Purpose**: Matches student questions to ground truth using cosine similarity
   - **Algorithm**: Greedy matching to avoid duplicate alignments

3. **Clinical Evaluation Engine**
   - **Model**: `emilyalsentzer/Bio_ClinicalBERT`
   - **Metrics**: BERTScore (Precision, Recall, F1)
   - **Specialization**: Medical terminology and clinical context understanding

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

## 📊 Real Evaluation Results

### Sample Output (from actual evaluation)

```json
{
  "results": [
    {
      "student_question": "hi, goerge, how you came today",
      "ground_truth_question": "hey george how are you today i understand you're here for some numbness and tingling in your fingers and some pain in your wrist",
      "patient_response": "I'm here because my wrist and hand have been hurting for a few months, and I've been having numbness and weakness, especially at night, which is really affecting my work as a landscaper.",
      "bertscore_precision": 0.7905,
      "bertscore_recall": 0.6965,
      "bertscore_f1": 0.7406
    },
    {
      "student_question": "what type of work you do, are you using your left hand mostly in your work chores",
      "ground_truth_question": "what kind of work do you do",
      "patient_response": "I'm a landscaper, so I use my hands a lot for physical labor, digging, pruning, and operating machinery, and my left hand is definitely more involved in these activities.",
      "bertscore_precision": 0.7585,
      "bertscore_recall": 0.8480,
      "bertscore_f1": 0.8008
    }
  ],
  "summary": {
    "average_precision": 0.7788,
    "average_recall": 0.7592,
    "average_f1": 0.7669,
    "total_questions": 5
  }
}
```

### Performance Insights

- **Average F1 Score**: 0.77 (Good clinical relevance)
- **Precision vs Recall**: Balanced performance indicating comprehensive question coverage
- **Medical Context Understanding**: Successfully captures clinical terminology and patient presentation

---

## 📁 Repository Structure

```
Medical-Chatbot-Evaluation-using-BERTScore/
├── scoring.ipynb                     # Main Jupyter notebook with full implementation
├── ground_truth_doctor_dialogues.json  # Expert dialogue reference for carpal tunnel case
├── chat_evaluation.json             # Sample evaluation results
└── README.md                        # Project documentation
```

---

## 🎯 Use Cases

### Medical Education
- **Clinical Skills Assessment**: Evaluate student interview techniques
- **Standardized Training**: Consistent evaluation across medical programs
- **Progress Tracking**: Monitor improvement in diagnostic questioning

### Research Applications
- **Dialogue Quality Analysis**: Quantify medical conversation effectiveness
- **AI Training Data**: Generate labeled datasets for medical AI development
- **Comparative Studies**: Benchmark different questioning approaches

### Healthcare Technology
- **Chatbot Evaluation**: Assess medical AI assistant performance
- **Training Simulation**: Develop realistic patient interaction scenarios
- **Quality Assurance**: Validate medical dialogue systems

---

## 🔬 Future Enhancements

### Technical Improvements
- [ ] Support for multiple medical conditions
- [ ] Real-time evaluation dashboard
- [ ] Integration with medical education platforms
- [ ] Multi-language support for global medical training

### Model Enhancements
- [ ] Fine-tuning on domain-specific medical dialogues
- [ ] Integration with latest clinical language models
- [ ] Custom scoring metrics for different medical specialties
- [ ] Automated ground truth generation

---

## 📚 References & Citations

1. **BERTScore Paper**: [BERTScore: Evaluating Text Generation with BERT](https://arxiv.org/abs/1904.09675)
2. **Bio_ClinicalBERT**: [Publicly Available Clinical BERT Embeddings](https://huggingface.co/emilyalsentzer/Bio_ClinicalBERT)
3. **LLaMA Model**: [HPAI-BSC/Llama3.1-Aloe-Beta-8B](https://huggingface.co/HPAI-BSC/Llama3.1-Aloe-Beta-8B)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

### Development Setup
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 👨‍💻 Author

**MeerBaloch7** - [GitHub Profile](https://github.com/MeerBaloch7)

For questions or collaboration opportunities, please open an issue in this repository.
