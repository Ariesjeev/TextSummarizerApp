# The Still — Text Summarizer

A small web app that turns long text into a concise summary. It uses a local
T5 sequence-to-sequence model behind a FastAPI API and includes a responsive
browser interface for entering text and viewing the result.

![The Still text summarizer interface](Screenshot%20(10).png)

## Features

- Summarizes text through a simple web interface.
- Exposes the summarizer as a JSON API.
- Uses PyTorch to select an available Apple MPS or NVIDIA CUDA device, falling
  back to CPU.
- Includes an interactive interface with input word and character counts.

## Requirements

- Python 3.10 or newer
- A compatible local T5 model in the `saved_summary_model` directory

The model weights are not included in this repository. The model file is about
242 MB, which exceeds GitHub's recommended limit for an individual file. Obtain
the model files separately and place them in `saved_summary_model` before
starting the app. The directory must include the model configuration, weights,
and tokenizer files (for example, `config.json`, `model.safetensors`,
`tokenizer.json`, and `tokenizer_config.json`).

## Setup

From the project directory, create and activate a virtual environment in
PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Install the Python dependencies:

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If you need a specific CUDA-enabled PyTorch build, install the version for your
system using the [official PyTorch installation selector](https://pytorch.org/get-started/locally/).

## Run the app

After placing the model files in `saved_summary_model`, run:

```powershell
python -m uvicorn app:app --reload
```

Open <http://127.0.0.1:8000> in your browser. FastAPI's interactive API
documentation is available at <http://127.0.0.1:8000/docs>.

## API

`POST /summarize/`

Request body:

```json
{
  "dialogue": "Paste the text you want summarized here."
}
```

Response:

```json
{
  "summary": "A concise summary of the submitted text."
}
```

The input is cleaned and truncated to the model's 512-token input limit. The
generated summary is limited to 150 tokens.

## Project structure

```text
.
├── app.py                 # FastAPI application and summarization logic
├── index.html             # Browser interface
├── requirements.txt       # Python dependencies
├── saved_summary_model/   # Local model files (not tracked by Git)
└── Screenshot (10).png    # Interface preview
```

## Notes

- Do not commit private data, credentials, or local environment files.
- `saved_summary_model/` is ignored by Git because it contains large model
  artifacts. A fresh clone needs the model files added locally before it can
  start.
- This project loads the model at application startup, so the model directory
  must be present before running Uvicorn.
