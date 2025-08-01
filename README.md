# Fine-tuning GPT-2 for LaTeX Generation
This project demonstrates how to fine-tune **GPT-2** on a small, handcrafted dataset to generate **LaTeX** code from natural language prompts. It’s a minimal but focused attempt to align an older language model with a specific instruction-following task — without requiring massive resources.

---
## 🧠 Project Overview

This project fine-tunes the `gpt2` model to translate **plain English prompts into LaTeX representations** — enabling the generation of **math expressions, equations, and symbols** in LaTeX format.

We train the model using a **handcrafted dataset of 50 English-to-LaTeX pairs**, designed to expose the model to structured and well-scoped instructions. Through instruction-style prompting and fine-tuning, GPT-2 learns to produce **syntactically accurate LaTeX outputs**, even though it was never originally trained for this domain.

✅ The entire training process — from **preprocessing and tokenization** to **fine-tuning and inference** — is handled within a single Jupyter notebook.  
✅ Built using Hugging Face’s `transformers` and `datasets` libraries for an end-to-end NLP pipeline.

🧪 Evaluation is conducted **qualitatively** using unseen prompts such as:
- `"write the LaTeX for x squared plus y squared equals z squared"`
- `"generate LaTeX for limit of x as x approaches 0"`

These are manually assessed to verify the **correctness and formatting** of the generated LaTeX code.

📊 The full pipeline includes:
- Fine-tuning GPT-2 on a **domain-specific LaTeX dataset**
- Prompt-based **English-to-LaTeX generation**
- **Saving and loading** the model for reuse
- Notebook-based training and result visualization

> 🎯 Goal: Demonstrate that even a **small, instruction-tuned dataset** can effectively repurpose a general language model like GPT-2 for **structured code generation tasks** — in this case, LaTeX math writing.

---

## 🛠️ Installation / Setup

To get started with fine-tuning GPT-2 for English-to-LaTeX translation, follow the steps below:

### 1. Clone the Repository
```bash
git clone https://github.com/Daddy-Myth/fine-tuning-gpt2-for-latex-generation.git
cd fine-tuning-gpt2-for-latex-generation
```
### 2. Create and Activate a Virtual Environment
```bash
# Create a virtual environment
python -m venv venv

# Activate it
venv\Scripts\activate
```
### 3. Install Required Dependencies
Install the necessary Python packages using **pip**:
```bash
pip install -r requirements.txt
```
### 4. Launch the Notebook
Run the Jupyter notebook to start the training process:

```bash
jupyter notebook
```
Then open Fine_tune_GPT2_LaTeX.ipynb and run through the cells sequentially to fine-tune and test the model.

---

## 🎯 Training Overview

The fine-tuning process is performed in a single Jupyter notebook and includes the following steps:

1. **Preprocessing the Dataset**
   - Load and clean the handcrafted English-to-LaTeX pairs.
   - Tokenize both inputs (plain English) and outputs (LaTeX code) using the GPT-2 tokenizer.

2. **Model Preparation**
   - Load the `gpt2` model from Hugging Face Transformers.
   - Resize the model embeddings to match the tokenizer's vocabulary (if extended).
   - Configure the model for sequence-to-sequence style generation.

3. **Fine-Tuning**
   - Train the model using a small number of epochs due to dataset size.
   - Track training loss with `tqdm` progress bars for easy monitoring.
   - Save the fine-tuned model locally for future inference.

4. **Evaluation**
   - Prompt the fine-tuned model with unseen English queries.
   - Generate corresponding LaTeX outputs.
   - Manually verify correctness of generated LaTeX syntax and structure.
---

### 🧪 Example Usage

Once the model is fine-tuned, you can generate LaTeX code by prompting it with plain English inputs like:

---

#### ✅ Prompt with Proper Formatting

**Prompt:**  
> write the LaTeX for x squared plus y squared equals z squared  
<img width="1138" alt="Prompt Example 1" src="https://github.com/user-attachments/assets/f5da3cbd-7858-4c3b-86da-61dbf91c22a5" />

---

**Prompt:**  
> generate LaTeX for limit of x as x approaches 0  
<img width="1234" alt="Prompt Example 2" src="https://github.com/user-attachments/assets/d3ecd6f7-509a-4a46-957d-a15d9d2a18d7" />


🧠 Both of the above examples use the exact conversion prompt format the model was trained on:
```python
conversion_text_sample = f'{CONVERSION_PROMPT}English: {text_sample}\n{CONVERSION_TOKEN}'
```

This ensures the instruction-tuned model stays aligned and outputs the desired LaTeX syntax with high accuracy — reflecting the consistent input-output mapping learned during training.

---

#### ⚠️ Improper Prompting Example

**Prompt:**  
> g of x equals integral from 0 to 1 of x squared  
<img width="1094" alt="Prompt Example 3 (Improper Format)" src="https://github.com/user-attachments/assets/2754a9af-b9ab-4ae3-8320-40188603eba0" />

When deviating from the expected format, the model struggles to generalize and may produce malformed or repetitive LaTeX code.

❗**Tip:** Always include the trained prefix (`English:`) and newline before the conversion token to get the best results.

---

💡 **Important Notes:**

- The **first two prompts** (`write the LaTeX...`, `generate LaTeX...`) were **exactly** taken from the training data. That’s why they produce clean and accurate LaTeX output.

- However, if the prompt format differs even slightly from what the model saw during training, the output can become inconsistent or contain unrelated tokens — especially **random characters or hallucinated text at the end**.

- This behavior stems from the model trying to continue the format it learned (e.g., continuing a LaTeX block), and can be improved with more training data, better prompt conditioning, or output post-processing.

---

## 📊 Evaluation & Performance

The fine-tuned model was tested on a range of English-to-LaTeX prompts to assess output quality and consistency.

---

### 🔍 Prompt Categories

| Prompt Type                  | Description                                                                 | Output Quality |
|-----------------------------|-----------------------------------------------------------------------------|----------------|
| ✅ **Seen Prompts**         | Exact prompts from the training dataset                                     | 🟢 Accurate    |
| ⚠️ **Similar Prompts**      | Slightly reworded inputs similar to training examples                        | 🟡 Partial     |
| ❌ **Unstructured Prompts** | Prompts with unusual phrasing or missing keywords                           | 🔴 Inaccurate  |

---

### 📌 Key Observations

- ✅ **Training Prompts** yielded **precise and clean LaTeX** (e.g., correctly formatted `\frac`, `\int`, etc.).
- 🧠 The model **generalizes decently** when the phrasing is close to seen examples.
- ❗ Output sometimes ends with **random trailing tokens** (e.g., stray brackets or characters), especially for novel prompts.
- 🔁 No quantitative metric (like BLEU or ROUGE) was used — evaluation was **qualitative** and based on visual accuracy.

---

### ⚠️ Limitations

-  Even correct prompts can sometimes result in outputs with **noise at the end**.
-  This is likely due to:
  - The model continuing a LaTeX-style completion.
  - Lack of post-processing.
  - Limited training data.

---

##  Acknowledgments

- 📚 [Quick Start Guide to LLMs](https://github.com/sinanuozdemir/quick-start-guide-to-llms) — the foundation and structure behind this project.
