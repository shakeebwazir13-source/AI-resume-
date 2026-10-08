# 📄 AI Resume ATS Analyzer

Upload a resume (PDF, DOCX or TXT) and get an **estimated ATS score**, a score
breakdown, missing keywords and concrete suggestions to improve it. Optionally
paste a job description for a tailored keyword match.

Built with **Streamlit** (UI) and **Google Gemini Flash** (AI).

## How the score works

| Component | Weight | Source |
|---|---|---|
| Keyword relevance | 30% | Gemini |
| Content & impact (action verbs, metrics) | 25% | Gemini |
| Structure & clarity | 20% | Gemini |
| ATS parseability (email, phone, Experience/Education/Skills headings, length, bullets) | 25% | Rule-based Python checks |

> The score is an **estimate**. Real ATS products don't publish a score, so use
> it as a guide to find weak spots, not as an exact prediction.

## Project structure

```
app.py            # Streamlit app
requirements.txt  # Python dependencies
README.md
```

## Run locally

1. Get a free Gemini API key: https://aistudio.google.com/apikey
2. Install and run:

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

export GEMINI_API_KEY="your-key"   # Windows PowerShell: $env:GEMINI_API_KEY="your-key"
streamlit run app.py
```

You can also skip the environment variable and paste the key into the sidebar.

### Changing the Gemini model

The default is `gemini-2.5-flash`. Google retires and releases models regularly,
so if you get a "model not found" error, check the current Flash model name at
https://ai.google.dev/gemini-api/docs/models and either type it in the sidebar
or set `GEMINI_MODEL`.

---

## Push to GitHub

1. Create an empty repository on https://github.com/new (e.g. `resume-ats-analyzer`).
   Do **not** tick "Add a README" - you already have one.
2. In your project folder, **protect your secrets first**:

```bash
printf "venv/\n__pycache__/\n.env\n.streamlit/secrets.toml\n" > .gitignore
```

3. Initialise, commit and push:

```bash
git init
git add .
git commit -m "Initial commit: AI Resume ATS Analyzer"
git branch -M main
git remote add origin https://github.com/<your-username>/resume-ats-analyzer.git
git push -u origin main
```

> If GitHub asks for a password, use a **Personal Access Token**
> (GitHub → Settings → Developer settings → Personal access tokens) or sign in
> with GitHub CLI (`gh auth login`). **Never commit your API key.**

## Deploy on Streamlit Community Cloud (free)

1. Go to https://share.streamlit.io and sign in with GitHub.
2. Click **Create app** → **Deploy a public app from GitHub**.
3. Choose your repository, branch `main`, and main file path `app.py`.
4. Open **Advanced settings → Secrets** and paste:

```toml
GEMINI_API_KEY = "your-gemini-api-key"
```

5. Click **Deploy**. After a minute or two you get a public URL like
   `https://<your-app>.streamlit.app`.

Any future `git push` to `main` redeploys automatically. To change the key
later: app menu → **Settings → Secrets**.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Very little text could be read" | The PDF is a scan/image. Export a text-based PDF or DOCX. |
| "model not found" / 404 | Update the model name in the sidebar (see above). |
| 429 / quota errors | You hit the free-tier rate limit - wait a minute and retry. |
| `ModuleNotFoundError` on deploy | Make sure `requirements.txt` is in the repo root. |

## Privacy

The resume text is sent to Google's Gemini API for analysis. Don't share a
public deployment holding your personal key with strangers - anyone using the
app spends your API quota. For public use, remove the secret and let each user
enter their own key in the sidebar.
