# Deploy Avon Lady to GitHub and Cloudflare Pages

Follow these steps to turn the **avon-lady** folder into its own Git repo, push it to GitHub, and host it on Cloudflare Pages.

---

## 0. Restore Voodoo's Attic in this repo (optional)

The main **voodoos-attic** folder may still contain the Avon Lady version in `index.html`. To restore the original Voodoo's Attic app in this repo, run in the **voodoos-attic** root (with `index.html` closed):

```powershell
cd "C:\Users\tarpl\OneDrive\My HTML Apps\VOODOOS ATTIC\voodoos-attic"
git checkout main -- index.html
```

Then commit if you want to keep that as the main version. The Avon Lady app lives in the **avon-lady** folder and is deployed separately.

---

## 1. Move Avon Lady to its own folder (if needed)

Right now **avon-lady** lives inside the **voodoos-attic** project. To use it as a separate app:

- **Option A:** Move the whole `avon-lady` folder to where you want the new project (e.g. same parent as voodoos-attic: `VOODOOS ATTIC\avon-lady`).
- **Option B:** Keep it where it is and run the commands below from inside `avon-lady` (then add `avon-lady/` to voodoos-attic’s `.gitignore` so it isn’t committed to the Voodoo repo).

---

## 2. Create a new GitHub repository

1. Go to [github.com](https://github.com) and sign in.
2. Click **New repository** (or **+** → **New repository**).
3. Set:
   - **Repository name:** e.g. `avon-lady` or `avon-lady-poster`
   - **Description:** optional (e.g. “Avon Lady Master Poster – Insta-ready video builder”)
   - **Public**
   - **Do not** add a README, .gitignore, or license (you already have these in `avon-lady`).
4. Click **Create repository**.
5. Copy the repo URL (e.g. `https://github.com/YOUR_USERNAME/avon-lady.git` or the SSH form).

---

## 3. Turn avon-lady into a Git repo and push to GitHub

Open a terminal in the **avon-lady** folder (the one that contains `index.html`), then run:

```powershell
cd "C:\Users\tarpl\OneDrive\My HTML Apps\VOODOOS ATTIC\voodoos-attic\avon-lady"
# Or, if you moved it:
# cd "C:\Users\tarpl\OneDrive\My HTML Apps\VOODOOS ATTIC\avon-lady"

git init
git add .
git commit -m "Initial commit: Avon Lady Master Poster"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/avon-lady.git
git push -u origin main
```

Replace `YOUR_USERNAME/avon-lady` with your actual GitHub username and repo name. If you use SSH:

```powershell
git remote add origin git@github.com:YOUR_USERNAME/avon-lady.git
```

---

## 4. Host on Cloudflare Pages

### 4a. Connect the GitHub repo

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) and sign in.
2. In the left sidebar, open **Workers & Pages**.
3. Click **Create** → **Pages** → **Connect to Git**.
4. Choose **GitHub** and authorize Cloudflare if asked.
5. Select the **avon-lady** repository (or whatever you named it).
6. Click **Begin setup**.

### 4b. Build settings

This project is static HTML (no build step), so use:

| Setting        | Value        |
|----------------|--------------|
| **Project name** | `avon-lady` (or any name you like) |
| **Production branch** | `main` |
| **Build command**    | Leave **empty** (or leave default and override below). |
| **Build output directory** | `/` or `.` (root) |

If Cloudflare shows “Build command” as required, set:

- **Build command:** `echo "No build"` or leave as is if it allows empty.
- **Build output directory:** `.` so it serves the root where `index.html` is.

Click **Save and Deploy**.

### 4c. Result

- Cloudflare will build and deploy. For a repo that only has static files, the “build” is very fast.
- When it’s done, you’ll get a URL like:  
  `https://avon-lady.pages.dev`  
  (or whatever project name you chose).
- Opening that URL will load your Avon Lady Master Poster app.

---

## 5. Optional: custom domain and favicons

- **Custom domain:** In the Cloudflare Pages project, go to **Custom domains**, add your domain, and follow the DNS instructions.
- **Favicons:** If you have `favicon-32x32.png`, `favicon-16x16.png`, `apple-touch-icon.png`, etc., add them to the **avon-lady** folder and commit + push; the app already references them in `index.html`.

---

## 6. Updating the site

After you change files in the **avon-lady** repo:

```powershell
cd "path\to\avon-lady"
git add .
git commit -m "Describe your change"
git push
```

Cloudflare Pages will automatically redeploy when you push to the connected branch (usually `main`).

---

## Summary

| Step | Action |
|------|--------|
| 1 | (Optional) Move `avon-lady` out of voodoos-attic; or ignore `avon-lady/` in voodoos-attic’s `.gitignore`. |
| 2 | Create a new **empty** GitHub repo for Avon Lady. |
| 3 | In the avon-lady folder: `git init`, `git add .`, `git commit`, `git remote add origin <repo-url>`, `git push -u origin main`. |
| 4 | In Cloudflare: **Workers & Pages** → **Create** → **Pages** → **Connect to Git** → select repo, output directory `.` (or `/`), deploy. |
| 5 | Use the provided `*.pages.dev` URL (and optionally add a custom domain and favicons). |

You then have:

- **voodoos-attic** – original app (separate repo, can stay as-is or also be put on Cloudflare).
- **avon-lady** – separate repo and site on GitHub + Cloudflare Pages for the Avon client.
