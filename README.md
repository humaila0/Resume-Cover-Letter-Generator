# Resume Generator (Gradio + Hugging Face Inference API)

A simple Gradio-based web app that generates professional resumes and cover letters from a job description and an existing resume. The app accepts plain text or PDF uploads for both the job description and the resume, uses a Hugging Face chat-capable model via the InferenceClient to generate content, and saves results as downloadable PDFs.

Live demo: https://huggingface.co/spaces/humaila/resume-generator-qwen

---

## Model used

This Space calls a chat-capable model via the Hugging Face InferenceClient. The model configured in the app is:

- model="mistralai/Mixtral-8x7B-Instruct-v0.1"

Note: You can change the model name in `app.py` to any compatible chat model available on Hugging Face Model Hub. Ensure the model you choose supports the chat/completions endpoint used by `huggingface_hub.InferenceClient`.

---

## Features

- Paste or upload a Job Description (PDF supported).
- Paste or upload an existing Resume (PDF supported).
- Generate:
  - Resume
  - Cover Letter
  - Both
- Output returned as text in the UI and downloadable PDF files.
- Uses the Hugging Face InferenceClient to call a chat-capable model.

---

## Project Structure

- `app.py` - Main application code (Gradio UI and generation logic).
- `requirements.txt` - Python dependencies.
- `.gitattributes` - Git attributes.
- `.git/`, `.venv/`, `.idea/` - local dev files (not required in repository; typically ignored).

---

## Requirements

- Python 3.8+
- A Hugging Face API token with inference access (set as `HF_TOKEN`)
- The Python packages in `requirements.txt` (see below)

Example `requirements.txt` (ensure the repo's `requirements.txt` includes these or similar):
```
gradio
huggingface-hub
PyPDF2
reportlab
```

---

## Installation (local)

1. Clone the repository:
   - git clone <your-repo-url>
   - cd resume-generator-qwen

2. Create and activate a virtual environment (recommended):
   - Linux / macOS:
     - python -m venv .venv
     - source .venv/bin/activate
   - Windows (PowerShell):
     - python -m venv .venv
     - .\.venv\Scripts\Activate.ps1

3. Install dependencies:
   - pip install -r requirements.txt

4. Set your Hugging Face token:
   - Linux / macOS:
     - export HF_TOKEN="hf_xxx..."
   - Windows (PowerShell):
     - $Env:HF_TOKEN="hf_xxx..."

   On Hugging Face Spaces, add `HF_TOKEN` as a Space secret via the web UI (Settings → Secrets).

5. Run the app:
   - python app.py

6. Open the Gradio UI:
   - By default Gradio prints a local URL and (optionally) a public share link.

---

## How it works (high level)

- `app.py` constructs a Gradio interface that accepts:
  - Job Description text or PDF
  - Resume text or PDF
  - A choice to generate Resume, Cover Letter, or Both
- PDF text extraction is handled by `PyPDF2`.
- The app calls a chat-enabled model through `huggingface_hub.InferenceClient` (configured to use `mistralai/Mixtral-8x7B-Instruct-v0.1` by default).
- Generated text is converted into simple PDF files using ReportLab and returned to the user.

---

## Configuration & Customization

- Model selection:
  - The model is set in `app.py` when creating chat completions. Change the model string to switch to another compatible model.
- Improve prompts:
  - The prompts used for resume and cover letter generation are inside `app.py`. Tweaking these prompts will change the style, length, and structure of outputs.
- PDF formatting:
  - The `save_to_pdf` function uses ReportLab with a minimal style. Modify it to change fonts, layout, or to add metadata.

---

## Deployment to Hugging Face Spaces

1. Create a new Space (Gradio runtime).
2. Push your repo to the Space (via git).
3. Add a Space secret named `HF_TOKEN` with your Hugging Face account token if your model requires authentication.
4. The Space will install dependencies from `requirements.txt` and start `app.py` automatically (or the file you set as entrypoint).

Notes:
- If you plan to use a model hosted on HF Model Hub which has rate limits or requires special permissions, ensure your token has sufficient access.
- Consider using smaller models for cost/performance if public spaces are under heavy use.

---

## Troubleshooting

- "Could not read PDF" errors:
  - Some PDFs (scanned images, encrypted PDFs) may not extract clean text with `PyPDF2`. You can add OCR (e.g., Tesseract) for scanned PDFs.
- Model/API errors:
  - Verify `HF_TOKEN` is set and valid.
  - Check that the chosen model supports chat completions; otherwise adapt the code to use the correct inference method.
- Long outputs truncated or incomplete:
  - Some models have token limits. Consider shortening prompts or using a larger-context model.

---

## Security & Privacy

- Do not commit your `HF_TOKEN` or other secrets to the repository.
- Be careful when uploading real resumes with sensitive personal information — if deploying publicly, inform users and handle uploads appropriately (and consider retaining files only temporarily).

---

## Example usage

1. Paste a Job Description into the JD box (or upload a JD.pdf).
2. Paste an existing resume or upload resume.pdf (optional).
3. Choose "Resume", "Cover Letter", or "Both" and click "Submit".
4. The app will generate text and provide downloadable PDFs for the selected outputs.

---

## Development & Contribution

Contributions are welcome. Typical improvements:
- Add OCR for scanned PDFs
- Improve styling of the generated PDFs
- Add multi-language support
- Use a tokenized prompt template with better control of sections

If you'd like help or want me to generate a PR with suggested improvements to prompts or PDF layout, tell me what you prefer.

---

## License

MIT License — feel free to reuse and adapt. Include a LICENSE file in the repo if you want to make the license explicit.

---

## Author / Reference

- Hugging Face Space: https://huggingface.co/spaces/humaila/resume-generator-qwen
- Original script: `app.py` (Gradio + Hugging Face InferenceClient)
