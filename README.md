# Resume ATS Checker

Upload a resume (PDF, DOCX or TXT) and get an estimated **ATS score**, a score breakdown, and specific improvements. Built with **Python + Streamlit** and the **Gemini Flash** model.

## What it does

- Reads your resume and extracts the text (the same text an ATS would see)
- Runs automated checks: email, phone, links, standard sections, length, bullets, measurable results, tables
- Asks Gemini to rate five areas and write feedback:

| Area | Weight |
|---|---|
| Keywords & relevance | 30% |
| Formatting & structure | 25% |
| Impact & achievements | 20% |
| Clarity & language | 15% |
| Completeness | 10% |

- Shows prioritised fixes, matched and missing keywords, and example bullet rewrites
- Optional: paste a job description to tailor the keyword analysis
- Download the full report as a `.md` file

> **Note:** Real ATS products don't publish a score and each behaves differently. The score here is an AI-based estimate. Use it as guidance, not a guarantee.

## Files

```
resume-ats-checker/
├── app.py
├── requirements.txt
└── README.md
```

## 1. Get a Gemini API key

1. Go to https://aistudio.google.com/apikey and sign in with your Google account.
2. Click **Create API key** and copy it.

## 2. Run it on your computer

Requires Python 3.10 or newer.

```bash
cd resume-ats-checker
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Create the file `.streamlit/secrets.toml` (make the `.streamlit` folder first):

```toml
GEMINI_API_KEY = "paste-your-key-here"
```

Start the app:

```bash
streamlit run app.py
```

It opens at http://localhost:8501.

## 3. Put it on GitHub

### Option A: in the browser (easiest)

1. Create an account at https://github.com if you don't have one.
2. Click **+ → New repository**. Name it `resume-ats-checker`, choose **Public** (or Private), and click **Create repository**.
3. Click **uploading an existing file**, drag in `app.py`, `requirements.txt` and `README.md`, and click **Commit changes**.
4. Click **Add file → Create new file**, name it `.gitignore`, paste the lines below, and commit:
   ```
   .streamlit/secrets.toml
   .venv/
   __pycache__/
   ```

### Option B: with Git

```bash
cd resume-ats-checker
git init
printf ".streamlit/secrets.toml\n.venv/\n__pycache__/\n" > .gitignore
git add .
git commit -m "Resume ATS checker"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/resume-ats-checker.git
git push -u origin main
```

Create the empty repository on GitHub first and replace `YOUR-USERNAME`. **Never upload your API key.** It goes only in Streamlit's Secrets box.

## 4. Deploy on Streamlit Community Cloud

1. Go to https://share.streamlit.io and sign in with GitHub.
2. Click **Create app** and choose to deploy from a GitHub repo.
3. Select your repository, the branch `main`, and set **Main file path** to `app.py`.
4. Click **Advanced settings**:
   - Set **Python version** to **3.12**.
   - In the **Secrets** box paste:
     ```toml
     GEMINI_API_KEY = "paste-your-key-here"
     ```
5. Click **Deploy**. The first build takes a few minutes.
6. To change the key later: open the app, **Manage app → Settings → Secrets**, save, then reboot the app.

Optional secrets:

```toml
GEMINI_MODEL = "gemini-2.5-flash"   # use another Gemini Flash model if you like
```

## How the file is read

| File type | Notes |
|---|---|
| PDF | Must be text-based. Scanned or image-only PDFs have no readable text, and an ATS can't read them either. |
| DOCX | Paragraphs and tables are read. Text in Word headers/footers is not. |
| TXT | Read as plain text. |

Maximum file size is 5 MB.

## Privacy

Your resume text (and job description, if you paste one) is sent to Google's Gemini API to produce the analysis. The app doesn't store files. Results are kept only in your browser session, and identical analyses may be cached on the server for up to one hour. Don't upload anything you aren't comfortable sharing. If the app is public, anyone with the link can use it and spends your Gemini quota, so consider making the repo private or restricting viewers in the app's **Share** settings.

## Troubleshooting

| Problem | Fix |
|---|---|
| `GEMINI_API_KEY is missing` | Add it to Secrets (cloud) or `.streamlit/secrets.toml` (local), then reboot. |
| "I couldn't find enough text in this file" | The PDF is scanned or image-based. Export a text-based PDF or DOCX from Word or Google Docs. |
| `Analysis failed: ... 429` / quota errors | You hit the free Gemini rate limit. Wait a minute and try again. |
| `Analysis failed: ... 404` / model not found | Set `GEMINI_MODEL` in Secrets to a Gemini model your key can use. |
| `Analysis failed: ... API key not valid` | Create a new key at https://aistudio.google.com/apikey and update Secrets. |
| `ModuleNotFoundError` on deploy | Check `requirements.txt` is in the repo root and the app was redeployed. |
| "Gemini returned an unreadable answer" | Click **Analyze resume** again. |

## Customising

- **Score weights and labels:** edit `SCORE_WEIGHTS` and `SCORE_LABELS` in `app.py`.
- **Instructions to Gemini:** edit `SYSTEM_PROMPT` in `app.py`.
- **Automated checks:** edit `run_local_checks()` in `app.py`.# resumeAtsScore
