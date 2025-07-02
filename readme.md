---
license: apache-2.0
tags:
  - codet5
  - sql-generation
  - text2sql
  - lora
  - peft
  - wikisql
library_name: transformers
pipeline_tag: text2text-generation
---

# 🚀 CodeT5-small LoRA fine-tuned on WikiSQL

This model is a **LoRA fine-tuned version** of [Salesforce/codet5-small](https://huggingface.co/Salesforce/codet5-small) for **natural language to SQL query generation** on the [WikiSQL](https://github.com/salesforce/WikiSQL) dataset.

It uses **PEFT (LoRA)** to adapt the base model efficiently with minimal extra parameters.  
Useful for learning and prototyping **text-to-SQL** tasks on simple table schemas.

---

## 📚 **Training Details**

- **Base Model:** `Salesforce/codet5-small`
- **Adapter:** LoRA (r=8, alpha=16) on attention `q` and `v` modules.
- **Dataset:** WikiSQL (21k train, 3k val)
- **Input Format:** `question: <QUESTION> table: <TABLE_HEADERS>`
- **Target:** Human-readable SQL query
- **Epochs:** 1–3 recommended for small runs.
- **Framework:** 🤗 Transformers + PEFT

---

## 🧩 **Example Usage**

```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

# Replace with your actual HF repo name
model_name = "Mahendra1742/SqlGPT"

tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSeq2SeqLM.from_pretrained(model_name)

# Example input
question = "How many employees are in the Marketing department?"
table = "| department | employees |"

prompt = f"question: {question} table: {table}"

inputs = tokenizer(prompt, return_tensors="pt")

outputs = model.generate(**inputs, max_length=128)

print("OUTPUT :- ")
print("   ")
print(tokenizer.decode(outputs[0], skip_special_tokens=True))

```

# Input
```
question: How many cities have a population over 1 million? table: | City | Population |

```

# Output
```
SELECT COUNT(*) FROM table WHERE Population > 1000000

```

