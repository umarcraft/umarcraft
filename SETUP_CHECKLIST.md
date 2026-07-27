# Setup Checklist — umarcraft/umarcraft

Everything in this zip is generated and ready. These are the manual steps left to go live.

## 0. Create the profile repo (skip if it already exists)
- Go to github.com/new
- Repository name: `umarcraft` (must exactly match your GitHub username)
- Public, check "Add a README file", Create

## 1. Push these files
In VS Code, open this folder, then in the terminal:
```bash
git init
git remote add origin https://github.com/umarcraft/umarcraft.git
git add .
git commit -m "Add animated profile banner, stats, snake, badges"
git branch -M main
git push -u origin main --force
```
(`--force` is only needed the first time if the repo already has an auto-created README you want to replace — otherwise just merge normally.)

This pushes: `README.md`, `dark.svg`, `light.svg`, `.github/workflows/snake.yml`.

## 2. Test the banner
- Open your profile: github.com/umarcraft
- Toggle GitHub's theme: avatar (top right) → Settings → Appearance → switch Light/Dark
- Reload — both `dark.svg` and `light.svg` should swap in automatically

## 3. Self-host your stats cards (~20 min, one-time)
The README currently points at `YOUR-INSTANCE.vercel.app` placeholders — replace these after deploying:

1. **Create a token**: github.com/settings/tokens → Tokens (classic) → Generate new token (classic)
   - Note: `readme-stats`, Expiration: No expiration, Scope: tick `repo`
   - Copy it immediately — GitHub shows it once. Never paste it into a chat or public repo.
2. **Fork**: github.com/anuraghazra/github-readme-stats
3. **Deploy**: vercel.com → Sign up with GitHub → Hobby (free) → Add New… → Project → import your fork → leave build settings alone
4. **Env variable**: in the Vercel project, add `PAT_1` = your token → Deploy → wait ~2 min
5. **Copy your URL**: `your-instance.vercel.app`
6. Verify: open `https://your-instance.vercel.app/api?username=umarcraft&show_icons=true` — a card should render
7. In `README.md`, replace both `YOUR-INSTANCE.vercel.app` occurrences with your real URL, then commit + push

## 4. Turn on the contribution snake
1. Repo → **Settings** (the repo's settings, not your account's) → sidebar **Actions** → **General** → scroll to **Workflow permissions** → select **Read and write permissions** → Save
   - URL should look like `github.com/umarcraft/umarcraft/settings/actions`
2. Push (already done in step 1) triggers the workflow automatically. Check the **Actions** tab — the run should go green in about a minute and create an `output` branch
3. The snake images in the README (`github-snake.svg` / `github-snake-dark.svg`) will only load correctly *after* that first green run — before that they'll show as broken images, which is expected
4. It regenerates every 12 hours automatically. To force it: Actions → Generate Snake Animation → Run workflow

## 5. Badges
LinkedIn and Email badges are already filled in with your links. If you get an Instagram, Facebook, or portfolio URL later, add them the same way — just don't set LinkedIn to a custom color (shields.io only renders the LinkedIn glyph on brand blue `#0A66C2`, which is already used here).

## Troubleshooting
| Symptom | Cause |
|---|---|
| Stats card shows "API rate limit exceeded" | Still pointing at the public instance — finish step 3 |
| Snake image broken | `output` branch doesn't exist yet — wait for the Action to run green |
| "Nothing changed" after an edit | Almost always CDN cache. Open the raw file URL with `?v=999` appended, or check you're looking at the right theme (dark assets only render in dark mode) |
| Banner looks slightly different than you expected | The portrait is generated from your photo via a dithering + segmentation pipeline — it's a one-off artistic rendering, not a filter preset, so minor asymmetries in the dot pattern are normal |

## Notes on what was simplified from the full "master prompt" spec
To keep the SVGs a reasonable file size (~390–400KB each, vs. the strategy's target of under ~1MB) and reliably renderable in a browser:
- Portrait dot count and the logo-morph "traveler" swarm were tuned down from the most extreme version of the spec (17k+ dots / 1,500-point swarm) — this was flagged in the source document itself as producing bloated files or a "loose impression" instead of a clean portrait.
- The three morphing logos (React, Django, Python) are procedurally drawn point-clouds rather than traced from official brand SVGs, matched dot-for-dot between shapes using the Hungarian algorithm (optimal assignment) so each point takes the shortest path — same technique the spec calls for, applied to simplified glyphs.
- If you want the logos to more precisely match the real brand marks, send reference SVGs/PNGs of the React atom, Django logo, and Python logo and I can re-trace the point clouds from those directly.
