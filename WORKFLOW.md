Dorami blog — Create new post workflow

Purpose

This document records the exact steps and commands used to create and publish a Dorami blog post with an AI-generated cover image, enforce face/outfit consistency, and publish safely to GitHub Pages. Keep it here so the flow is repeatable.

Principles

- Keep identity consistent: dorami-santorini.png is the canonical face reference. Use it whenever identity continuity is required.
- Keep outfit consistent: dorami-sunrise-coffee.png for daytime outfits; dorami-greek-dinner.png for evening/night outfits.
- Use a two-reference edit workflow for generation: first pass in the outfit/composition reference, second pass to fix the face with the canonical face reference.
- Archive high-resolution masters externally (Git LFS or local archive) and keep the repository for the published images.
- Prefer non-destructive workflow: publish to a gh-pages branch or a separate repo for testing before replacing a live user/org site.

Files & layout

- site/dorami/
  - assets/ — images used by the site (these should be committed to git so Pages can serve them)
  - posts/ — one HTML file per post (full post HTML)
  - posts.json — manifest listing posts (newest-first)
  - index.html — renders the feed
  - WORKFLOW.md — this file

Image generation (azure-image-gen)

1) Generate the composition image (edit mode if you want to preserve a look):

python3 skills/azure-image-gen/scripts/gen.py "<prompt: scene>" --edit --ref site/dorami/assets/<outfit-or-scene-ref>.png

2) If the face drifts, run a targeted face-replace edit with two refs (composition then face):

python3 skills/azure-image-gen/scripts/gen.py "Replace Dorami's face in the image to match the face/appearance in dorami-santorini.png while keeping the composition and lighting." --edit --ref site/dorami/assets/dorami-<candidate>.png site/dorami/assets/dorami-santorini.png

3) From the script output (skills/azure-image-gen/scripts/out/<ts>.png):
  - Archive the high-resolution original in your private archive folder (e.g., /home/thomas/.openclaw/images) or Git LFS if you prefer.
  - Save the final image file directly into site/dorami/assets/dorami-<slug>.png (committed to the repo). Do not require an additional web-optimization step — keep the master you want published as the file in the assets folder.

Naming

- Keep filenames descriptive and stable: dorami-<slug>.png (slug matches post filename prefix). Avoid renaming accepted, published files — instead add a new file and update posts.json.

Creating a post

1) Create the post HTML at site/dorami/posts/YYYY-MM-DD-<slug>.html. Use the standard post template: title, meta date, <img src="../assets/dorami-<slug>.png">, and body.

2) Update posts.json by PREPENDING the new entry (newest-first). Example Python snippet:

python3 - <<'PY'
import json
p='site/dorami/posts.json'
with open(p,'r',encoding='utf-8') as f:
  posts=json.load(f)
new={
  "title": "Post Title",
  "date": "YYYY-MM-DD",
  "slug": "YYYY-MM-DD-<slug>",
  "path": "posts/YYYY-MM-DD-<slug>.html",
  "cover": "assets/dorami-<slug>.png",
  "excerpt": "Short excerpt"
}
posts.insert(0,new)
with open(p,'w',encoding='utf-8') as f:
  json.dump(posts,f,ensure_ascii=False,indent=2)
print('updated')
PY

Publishing & Pages

Recommended safe flows:

- Option A (recommended): Push a prebuilt gh-pages branch containing the full site tree (index, dorami/, assets/) and set GitHub Pages to serve from gh-pages / root. This avoids Pages build-time LFS issues.

- Option B: Serve from main/root but ensure the published branch contains the binary image files in the expected paths.

To trigger a rebuild after pushes, a small no-op commit (touching a file) forces Pages to rebuild if needed:

cd site
echo $(date) > dorami/_rebuild_trigger.txt
git add dorami/_rebuild_trigger.txt && git commit -m "chore: trigger Pages rebuild" && git push

Image handling

- Save the image you want to publish directly to site/dorami/assets (do not rely on a separate web-optimization step).
- Archive high-resolution masters outside the repo (e.g., /home/thomas/.openclaw/images) or in Git LFS if desired.

Troubleshooting

- If images are 404 on the published site but exist in the repo: ensure the published branch contains the actual binary files (not LFS pointers) and files are in the expected paths (assets/ and posts/).
- If GitHub API calls fail while automating Pages changes, confirm your GH token has the required scopes (repo, delete_repo when needed, and workflow/pages admin scopes).

This file is the authoritative workflow for Dorami posts. Update it as the process improves.
