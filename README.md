<p align="center">
  <img src="Assets/App_icon.png" alt="Project Logo" width="200"/>
</p>

<p align="center">
  <a href="https://www.python.org/"><img alt="Python" src="https://img.shields.io/badge/python-3.11-blue"></a>
  <a href="https://choosealicense.com/licenses/gpl-3.0/"><img alt="License: GPL v3" src="https://img.shields.io/badge/License-GPLv3-yellow.svg"></a>
  <a href="https://buymeacoffee.com/leonardo.corsini"><img alt="Make a donation" src="https://img.shields.io/badge/Make a donation-red.svg"></a>
  
</p>

---
<h1 align="center">
Vermanent: Verba <del>volant scripta</del> manent
</h1>



<h4 align="center">
Find clues in voice messages
</h4>

<p align="center">
<img src="Assets/Vermanent_demo.gif" width="800">
</p>

---

## About
Vermanent is a tool designed to address the significant challenge of analyzing a large number of voice messages 
in a smartphone forensic copy, combining speech-to-text and word embeddings to perform a textual similarity search in audio transcripts.
It works locally to guarantee data privacy, and it makes use of an intuitive interface to manage more than one case at a time.

---
## Features 

- Search across transcripts using semantic similarity
- Analyze voice messages from a directory or a zip, tar, gz file
- Uses spaCy or custom embeddings
- Works locally, no data leaves your machine
- GPU acceleration for transcriptions via CUDA-supported PyTorch

---

## Installation 

Requirements

- Python 3.11 
- pip, tkinter, ffmpeg 
- Updated NVIDIA drivers (only for GPU mode) 

To install the app in your local environment, follow these steps:

### Clone the repository

```
git clone https://github.com/Leonardo-Corsini/Vermanent.git
cd Vermanent
```

### Create and activate a virtual environment

```
#to create it
python3 -m venv .venv

#to activate it
#linux
source .venv/bin/activate

#windows
.venv\Scripts\activate
```
### Install dependencies
```
pip install --upgrade pip

pip install -r requirements.txt

# torch installed separately with CUDA support
You can chose the right installation command depending on your cuda architecture here: https://pytorch.org/
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```
Whisper needs ffmpeg to work properly,
To install it you can follow these steps or follow the steps described in ffmpeg official page (https://ffmpeg.org/):

```
#linux
sudo apt install ffmpeg

#Windows (in powershell with admin rights)
choco install ffmpeg
```

This app rely on external embedding models to work properly, that are differently licensed from this code (See Third-party content section).

If you want to use spacy trained pipelines you can run the script [install_spacy_models.py](install_spacy_models.py) to automatically download them with this command (only spacy models will be automatically downloaded):

```
python install_spacy_models.py
```
The italian spacy pipeline has a CC-BY-NC-SA licence, and it's not compatible with the licence of this code (GNU GPL 3.0). If you want to add an italian model you can follow the steps described in the "Make use of custom word embeddings models" section, using FastText it model, that is more permissive.

To run the application go in Vermanent folder and activate virtual environment.
Then run:

```
python main.py
```

---
## Make use of custom word embedding models

To make your chosen model work, follow these steps (fasttext model is used as an example):

- Download fasttext model with only vectors here (not bin file): https://fasttext.cc/docs/en/crawl-vectors.html

- Initialize the model. Model path should be in the directory "Vermanent\search\search_models\".

To initialize the model, run this command ([lang] has to be an ISO 639 language code of the set 1, which you can find here: https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes):
```
python -m spacy init vectors [lang] "path\to\cc.[lang].300.vec" "Vermanent\search\search_models\[lang]"
```

- Then update [languages.json](search%2Flanguages.json) file.
You have to add a new language field if it does not exist or modify an existing one.
Add this to the json list:
```
"[lang]": {
    "model": "search\search_models\[lang]",
    "spacy": false
  },
```
It is also recommended to rerun the install_spacy_models.py script to automatically download the correct stanza pipeline.

---

## Usage
### Case creation
- You can only use alphanumeric chars, "-", and "_" to assign a name to a case.
- When selecting the folder where the evidence to be analysed is contained, it is not necessary that the folder contains only audio files. Vermanent will automatically select only files of interest, even within .zip, .tar, or .gz archives.
- You can make several transcriptions and searches at a time, but the overhaul performance depends on your machine, so it's raccomanded to not overload too much the system.

### Transcription 

- With Vermanent you can choose the Whisper model that best suits your needs, but only when starting the app. Until the next restart, the app will use the previously loaded model.

- It is recommended to use an accurate transcription model in order not to affect the quality of the search, but if large model is too slow, the turbo model could be a great solution.

- Vermanent works with GPU with CUDA architectures.

### Search 
- Vermanent search process works with several languages, but for now only one at a time.
- You can search for multiple words, sentences, or single words. 
- Search use only CPU for now, but GPU compatibility is in development. This could take couple of minutes for each search depending on the system and the number of data.

## Support

If you find this project useful, and you want to support my work, consider giving it a ⭐ on GitHub, sharing it with others.
You could donate on https://buymeacoffee.com/leonardo.corsini to support the development. I will be grateful for any donation you want to make.

Contributions and feedback are welcome!

## License 

This code is licensed under the GNU General Public License v3.0 (GPL-3.0), which allows modification and redistribution under the same terms.  
For full details, see the [LICENSE](./LICENSE) file.

## Model Attributions

This file lists third-party NLP models that may be downloaded automatically by the application.
Each model is licensed separately and not covered by the GPL license of this repository.

| Language | Model                           | Author    | License      |
|----------|---------------------------------|-----------|--------------|
| `en`     | `en_core_web_lg`                | Explosion | MIT          |
| `zh`     | `zh_core_web_lg`                | Explosion | MIT          |
| `fr`     | `fr_core_news_lg`               | Explosion | LGPL-LR      |
| `de`     | `de_core_news_lg`               | Explosion | MIT          |
| `es`     | `es_core_news_lg`               | Explosion | GNU GPL 3.0  |
| `ro`     | `ro_core_news_lg`               | Explosion | CC BY-SA 4.0 |
| `ru`     | `ru_core_news_sm`               | Explosion | MIT          |
| `sl`     | `sl_core_news_lg`               | Explosion | CC BY-SA 4.0 |
| `-`      | `fasttext (e.g., cc.*.300.vec)` | Meta      | CC BY-SA 3.0 |


## Disclaimer

The use of this software is entirely at the user’s own risk. 
The author accepts no liability for any direct or indirect consequences resulting from its use, including but not limited to unauthorized access, violation of privacy laws, or improper handling of data. 
It is the user’s sole responsibility to ensure that any use of this tool complies with all applicable laws and regulations in their jurisdiction.
