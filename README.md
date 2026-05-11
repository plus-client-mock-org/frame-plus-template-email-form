# Frame+ Email Form Template

A minimal vanilla HTML + JavaScript template for Frame+.

## What it does
- Shows one email input field
- Has a submit button
- On submit, posts JSON to `POST /frame+/submit`
- Sends the Frame+ bearer token from the `/frame+/{urlToken}` path when available
- Shows inline success and error feedback

## Run locally
Open `index.html` directly in your browser.

## Push to GitHub
```powershell
git init
git add .
git commit -m "Add simple Frame+ email form template"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```
