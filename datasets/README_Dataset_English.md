# Silero-VAD Dataset

> This dataset was created with the support of the Innovation Promotion Fund under the federal project **“Artificial Intelligence”** of the **National Program “Digital Economy of the Russian Federation.”**

The links below provide **`.feather`** files containing **Silero VAD-labeled** open-source audio datasets, along with brief descriptions and loading examples.  

`.feather` files can be loaded using **pandas**:

```python
import pandas as pd
dataframe = pd.read_feather(PATH_TO_FEATHER_FILE)
```

---

## 📄 Dataset Structure

Each `.feather` annotation file contains:

- **`speech_timings`** – List of speech segments for the audio file.  
  Format: `{'start': START_SECOND, 'end': END_SECOND}` (in seconds).  
  The number of entries equals the number of detected speech segments.

- **`language`** – ISO language code of the audio.

> **All data is annotated with a temporal resolution of ~30 ms (`num_samples` = 512)**

Additional columns providing audio file locations vary per dataset and are described below.

---

## 📊 Dataset Overview

| Dataset Name           | Hours   | Languages | Link | License | md5sum |
|------------------------|--------:|---------:|------|---------|--------|
| **Bible.is**             | 53,138  | 1,596     | [URL](https://live.bible.is/)                 | [Custom](https://live.bible.is/terms)       | ea404eeaf2cd283b8223f63002be11f9 |
| **globalrecordings.net** | 9,743   | 6,171[^1] | [URL](https://globalrecordings.net/en)       | CC BY-NC-SA 4.0                             | 3c5c0f31b0abd9fe94ddbe8b1e2eb326 |
| **VoxLingua107**         | 6,628   | 107       | [URL](https://bark.phon.ioc.ee/voxlingua107/) | CC BY 4.0                                  | 5dfef33b4d091b6d399cfaf3d05f2140 |
| **Common Voice**         | 30,329  | 120       | [URL](https://commonvoice.mozilla.org/en/datasets) | CC0                                  | 5e30a85126adf74a5fd1496e6ac8695d |
| **MLS**                  | 50,709  | 8         | [URL](https://www.openslr.org/94/)           | CC BY 4.0                                  | a339d0e94bdf41bba3c003756254ac4e |
| **Total**                | **150,547** | **6,171+** |      |         |        |

---

## 📂 Dataset Details

### Bible.is
[`.feather` annotation file](https://models.silero.ai/vad_datasets/BibleIs.feather)

- **`audio_link`** – Direct links to audio files.

---

### globalrecordings.net
[`.feather` annotation file](https://models.silero.ai/vad_datasets/globalrecordings.feather)

- **`folder_link`** – Links to `.zip` archives for specific languages.  
  ⚠️ Links are repeated because each archive may contain multiple audio files.

- **`audio_path`** – Path to individual audio files **after** extraction.

---

### VoxLingua107
[`.feather` annotation file](https://models.silero.ai/vad_datasets/VoxLingua107.feather)

- **`folder_link`** – Links to `.zip` archives per language (duplicates exist for multi-file archives).  
- **`audio_path`** – Path to extracted audio files.

---

### Common Voice
[`.feather` annotation file](https://models.silero.ai/vad_datasets/common_voice.feather)

- This dataset **cannot** be downloaded via static links.  
  To obtain the data:
  1. Visit [Common Voice datasets](https://commonvoice.mozilla.org/en/datasets)  
  2. Request access via the official form  
  3. Download archives for all available languages

- **`audio_path`** – Unique `.mp3` filenames after downloading.  
- The annotations correspond to **Common Voice Corpus 16.1**.

---

### MLS
[`.feather` annotation file](https://models.silero.ai/vad_datasets/MLS.feather)

- **`folder_link`** – Links to `.zip` archives per language (duplicates exist for multi-file archives).  
- **`audio_path`** – Path to extracted audio files.

---

## 📜 License

This dataset is distributed under the  
[**CC BY-NC-SA 4.0**](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en) license.

---

## 📖 Citation

```bibtex
@misc{Silero VAD Dataset,
  author = {Silero Team},
  title = {Silero-VAD Dataset: a large public Internet-scale dataset for voice activity detection for 6000+ languages},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/snakers4/silero-vad/datasets/README.md}},
  email = {hello@silero.ai}
}
```

[^1]: The number of unique ISO codes does not match the number of languages because some closely related languages may share the same ISO code.
