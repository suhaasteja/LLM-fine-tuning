# LLM-fine-tuning

## 🎸 Pink Floyd Lyric Generator (Llama-3.1 Fine-Tune)

This project fine-tunes **Meta's Llama-3.1-8B** model to write song lyrics in the style of **Pink Floyd**. It uses the **Unsloth** library for efficient, 4-bit QLoRA training, allowing it to run on free Google Colab GPUs.

## 📝 Project Overview

* **Goal:** Create an AI that generates abstract, cynical, or psychedelic lyrics based on a user's prompt and style constraints.
* **Base Model:** `unsloth/Meta-Llama-3.1-8B-Instruct-bnb-4bit`
* **Technique:** QLoRA (Quantized Low-Rank Adaptation)
* **Dataset Format:** Alpaca (`Instruction`, `Input`, `Output`)

## ⚙️ Workflow Steps

### 1. Data Collection

* Loads raw Pink Floyd lyrics from **Kaggle** (`joaorobson/pink-floyd-lyrics`).

### 2. Dataset Preparation (Synthetic Tagging)

* Since raw lyrics don't have "prompts," we used an LLM (GPT-4o) to reverse-engineer training data.
* For each song, we generated:
* **Instruction:** A natural language request (e.g., "Write a song about the emptiness of fame").
* **Input:** Style tags (e.g., "Mood: Cynical, Style: Acoustic Ballad").
* **Output:** The original lyrics.


* *Result:* Saved as `pink_floyd_alpaca_responses_api.json`.

### 3. Training (Fine-Tuning)

* **Library:** Uses `unsloth` for 2x faster training and 60% less memory usage.
* **Config:**
* Rank (`r`): 16
* Batch Size: 2 (Gradient Accumulation: 4)
* Max Steps: 60 (for demo) / 300+ (for full convergence)


* **Outcome:** The model learns to mimic the vocabulary, rhyme schemes, and thematic elements of Roger Waters and David Gilmour.

### 4. Inference & Testing

* Compares the **Base Model** (generic poetic output) vs. the **Fine-Tuned Model** (Pink Floyd style).
* **Example Prompt:** *"Write a song about artificial intelligence"*
* **Style Input:** *"Style: Dark, Mood: Foreboding"*

## 🚀 How to Run

1. **Install Dependencies:** Run the Unsloth installation cell (optimized for Colab).
2. **Load Data:** Mount Google Drive to load the pre-generated `pink_floyd_alpaca_responses_api.json`.
3. **Train:** Execute the `SFTTrainer` cell.
4. **Generate:** Use the inference cell to create new lyrics.

## 📂 Files

* `Fine tune Pink floyd.ipynb`: The main training notebook.
* `pink_floyd_alpaca_responses_api.json`: The training dataset.
* `lora_model/`: The saved Low-Rank Adapters (the "learned" part of the model).
