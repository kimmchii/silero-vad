# Fine-Tuning Silero-VAD

> This fine-tuning code was developed with the support of the Innovation Promotion Fund under the federal project **“Artificial Intelligence”** of the **National Program “Digital Economy of the Russian Federation.”**

Fine-tuning is used to improve the speech detection quality of the **Silero-VAD** model on custom datasets.

---

## 📦 Dependencies

Make sure the following dependencies are installed:

- `torch >= 1.12.0`
- `torchaudio >= 0.12.0`
- `omegaconf >= 2.3.0`
- `sklearn >= 1.2.0`
- `pandas >= 2.2.2`
- `tqdm`

---

## 📂 Data Preparation

Training and validation datasets must be stored as **`.feather`** files with the following required columns:

- **`audio_path`** – Absolute path to the audio file on the filesystem.  
  - Audio must be in **PCM** format, preferably `.wav` or `.opus` (other popular formats are also supported).  
  - For faster fine-tuning, pre-resample all audio to **16,000 Hz**.

- **`speech_ts`** – Speech segment annotations for the corresponding audio file.  
  - Format: a list of dictionaries `{'start': START_SEC, 'end': END_SEC}`, where times are in **seconds**.  
  - Use annotations with **≤30 ms precision** for the best fine-tuning results.

> **Tip:**  
> - The more training data you provide, the better the model adapts to your domain.  
> - Audio length is not restricted.  
> - Each file will be **trimmed to `max_train_length_sec`** before feeding into the model.  
> - Long recordings should be **pre-split** into segments of this length for efficiency.

An example dataset is provided in **`example_dataframe.feather`**.

---

## ⚙️ Configuration (`config.yml`)

The configuration file specifies dataset paths and fine-tuning parameters:

| Parameter               | Description                                                                                          |
|-------------------------|------------------------------------------------------------------------------------------------------|
| `train_dataset_path`    | Absolute path to the training `.feather` DataFrame with `audio_path` and `speech_ts` columns.          |
| `val_dataset_path`      | Absolute path to the validation `.feather` DataFrame.                                                |
| `jit_model_path`        | Absolute path to the `.jit` Silero-VAD model. If empty, the model will be loaded according to `use_torchhub`. |
| `use_torchhub`          | `True` to load via **`torch.hub`**, `False` to use the `silero-vad` library (`pip install silero-vad`). |
| `tune_8k`               | `True` to fine-tune the **8 kHz** head, `False` for **16 kHz**.                                       |
| `model_save_path`       | Path where the fine-tuned model will be saved.                                                        |
| `noise_loss`            | Loss coefficient for non-speech audio windows.                                                       |
| `max_train_length_sec`  | Maximum audio duration in seconds for training (longer files will be truncated).                      |
| `aug_prob`              | Probability of applying audio augmentations during fine-tuning.                                      |
| `learning_rate`         | Learning rate for fine-tuning.                                                                       |
| `batch_size`            | Training and validation batch size.                                                                  |
| `num_workers`           | Number of data loader worker threads.                                                                |
| `num_epochs`            | Number of fine-tuning epochs (1 epoch = 1 pass over the training data).                              |
| `device`                | `cpu` or `cuda`.                                                                                     |

---

## 🚀 Fine-Tuning

Start fine-tuning with:

```bash
python tune.py
```

- Training runs for `num_epochs`.  
- The **best checkpoint** (based on ROC-AUC on the validation dataset) will be saved to `model_save_path` in **`.jit`** format.

---

## 🎯 Threshold Search

After fine-tuning, you can find the **optimal VAD thresholds**:

```bash
python search_thresholds
```

- Uses the same configuration file as fine-tuning.  
- Applies the trained model to the **validation dataset** to find the best input/output thresholds.

---

## 📖 Citation

If you use this repository in your work, please cite it as:

```bibtex
@misc{Silero VAD,
  author = {Silero Team},
  title = {Silero VAD: pre-trained enterprise-grade Voice Activity Detector (VAD), Number Detector and Language Classifier},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/snakers4/silero-vad}},
  commit = {insert_some_commit_here},
  email = {hello@silero.ai}
}
```
