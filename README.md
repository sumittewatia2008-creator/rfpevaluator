# EV Battery Supplier RFP Evaluator

A Streamlit app for importing supplier responses and evaluating them against
the EV battery RFP rubric. It supports PDF, DOCX, TXT, and Markdown uploads,
offline keyword triage, and optional AI evaluation with the Anthropic API.

## Run locally

Requires Python 3.10 or newer.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
streamlit run app.py
```

The app opens in your browser. AI evaluation requires an Anthropic API key;
enter it in the app's sidebar. The offline heuristic mode works without a key.

## Deploy on Streamlit Community Cloud

1. Push this project to a GitHub repository. Include `app.py`,
   `requirements.txt`, `README.md`, `.gitignore`, and the `PDF documents`
   folder if you want the sample documents available in the repository.
2. Sign in to [Streamlit Community Cloud](https://share.streamlit.io/) using
   GitHub and authorize access to the repository.
3. Create an app, select the repository and branch, and set the main file path
   to `app.py`.
4. Deploy. Streamlit Cloud installs the packages listed in
   `requirements.txt`.
5. To use AI evaluation, enter your Anthropic API key in the app's sidebar.
   Do not commit API keys or other secrets to GitHub. The app keeps the key in
   Streamlit session state and does not store it in its database.

Offline heuristic mode is available after deployment without configuring an
Anthropic API key.

## Publish to GitHub

For a new, empty repository, run these commands from this project directory,
replacing the URL with your repository URL:

```powershell
git init
git add app.py requirements.txt README.md .gitignore "PDF documents"
git commit -m "Prepare EV RFP evaluator for deployment"
git branch -M main
git remote add origin https://github.com/<your-account>/<your-repository>.git
git push -u origin main
```

If the GitHub repository already has a commit (for example, an initial
`.gitattributes` file), clone it into a separate folder first so that commit is
preserved. From this project directory, run:

```powershell
git clone https://github.com/<your-account>/<your-repository>.git ..\rfpevaluator-github
Copy-Item app.py, requirements.txt, README.md, .gitignore -Destination ..\rfpevaluator-github
Copy-Item "PDF documents" -Destination ..\rfpevaluator-github -Recurse
Set-Location ..\rfpevaluator-github
git add app.py requirements.txt README.md .gitignore "PDF documents"
git commit -m "Prepare EV RFP evaluator for deployment"
git push
```

If Git asks for your identity, configure `user.name` and `user.email` locally
before committing. If the project is already a Git repository, add, commit, and
push the deployment files using its existing remote instead.

## Data and deployment notes

The app stores imported responses, evaluation runs, and other working data in a
local SQLite database (`rfp_evaluator.db` by default). This database is ignored
by Git and is not included in the deployment. Streamlit Community Cloud's local
filesystem should not be treated as durable storage, so data stored by a
deployed instance may not survive a restart or redeploy. Use this deployment
for demos unless you add a persistent database for production use.
