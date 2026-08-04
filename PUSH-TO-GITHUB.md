# Push this project to GitHub (github.com/selvasmallive)

These files can't be pushed from the assistant's sandbox — GitHub is network-blocked there and no GitHub connector is installed. Run the steps below **on your own machine**, where GitHub is reachable. Everything is already prepared (a `.gitignore` is in place); you just need to init, commit, and push.

Open a terminal (PowerShell) and `cd` into this folder:

```powershell
cd "G:\My Drive\falconexam\falconexam-claude-development-kit\falconexam-development-kit"
```

## Option A — GitHub CLI (one command creates the repo and pushes)

Requires the GitHub CLI (`gh`) installed and authenticated (`gh auth login`).

```powershell
git init
git add .
git commit -m "Initial commit: FalconExam Claude Development Kit"
git branch -M main
gh repo create selvasmallive/falconexam-development-kit --private --source=. --remote=origin --push
```

Change `--private` to `--public` if you want a public repo, and rename `falconexam-development-kit` if you prefer a different repo name.

## Option B — Manual (create the repo in the browser first)

1. Go to https://github.com/new, owner **selvasmallive**, name **falconexam-development-kit**, and create it **empty** (no README/.gitignore/license).
2. Then run:

```powershell
git init
git add .
git commit -m "Initial commit: FalconExam Claude Development Kit"
git branch -M main
git remote add origin https://github.com/selvasmallive/falconexam-development-kit.git
git push -u origin main
```

If prompted for credentials, use a GitHub **Personal Access Token** (fine-grained, scoped to this one repo, `Contents: Read and write`) as the password. Do not paste tokens into this chat.

## Note on repository layout

The kit's own `README.md` describes the kit living under `/development-kit` inside your future FalconExam application repo. Two reasonable choices:

- **Kit as its own repo (what these commands do):** push this folder as `falconexam-development-kit`. Simple, and fine for now.
- **Kit inside the app repo:** when you scaffold the application (via `prompts/00-master-architect.md`), create the FalconExam app repo and move this kit into `development-kit/` at its root, as the README instructs.
