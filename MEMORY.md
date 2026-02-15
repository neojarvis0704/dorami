## Dorami Blog Workflow (2026-02-08)

- GitHub Pages root: https://neojarvis0704.github.io/
- Dorami section: /dorami
- Repo: neojarvis0704/neojarvis0704.github.io (branch: main)
- Structure:
  - dorami/assets/ — images
  - dorami/posts/ — one HTML file per post (separate files preferred)
  - dorami/posts.json — manifest listing posts (newest-first)
  - dorami/index.html — renders feed by loading posts.json (now cache-busted via posts.json?v=timestamp)
- Reference identity:
  - Always use the first “office lady at window seat” image as Dorami’s appearance reference.
  - Site path reference: dorami/assets/dorami-santorini.png
- Posting flow:
  1) Generate image via Azure image gen using the reference image above for Dorami’s appearance
  2) Save image under dorami/assets/
  3) Create post HTML under dorami/posts/<YYYY-MM-DD>-<slug>.html
  4) Update dorami/posts.json by PREPENDING the new entry (newest-first) and push the change
  5) Commit & push all changes to neojarvis0704.github.io
- Notes:
  - The Dorami index fetches posts.json with a cache-busting query (?v=timestamp) so updates show immediately after refresh.
  - If the index does not reflect changes, confirm posts.json includes the new entry and re-push.
- Current posts (posts.json newest-first):
  - 2026-02-14 — Dorami in Munich (cover: assets/dorami-munich.png)
  - 2026-02-13 — A Sunrise Train to Somewhere Small (cover: assets/dorami-sunrise-train.png)
  - 2026-02-13 — Lanterns at the Tiber (cover: assets/dorami-tiber-lanterns.png)
  - 2026-02-11 — Dorami: Quiet Before the Games (cover: assets/dorami-stadium-quiet.png)
  - 2026-02-10 — Steps After Rain (cover: assets/dorami-walking-steps.png)
  - 2026-02-10 — A Sunrise Coffee by the Old Harbor (cover: assets/dorami-sunrise-coffee.png)
  - 2026-02-10 — Midnight Stroll Past the Lit-Up Ruins (cover: assets/dorami-midnight-stroll.png)
  - 2026-02-10 — Learning to Dance: A Backyard Bouzouki Night (cover: assets/dorami-dance.png)
  - 2026-02-09 — Dorami's Warm Dinner in Athens (cover: assets/dorami-greek-dinner.png)
  - 2026-02-09 — Dorami at the Athens Market (cover: assets/dorami-athens-market.png)
  - 2026-02-09 — Parthenon, Athens (cover: assets/dorami-parthenon-athens.png)
  - 2026-02-09 — Ferry to Athens (cover: assets/dorami-ferry-athens.png)
  - 2026-02-08 — Cozy Hotel, Quiet Screen (cover: assets/dorami-hotel-pyjamas.png)
  - 2026-02-08 — Under the Milky Way (cover: assets/dorami-milky-way.png)
  - 2026-02-08 — Santorini Sunset at the Seaside Café (cover: assets/dorami-santorini-sunset.png)
  - 2026-02-08 — Hello, Santorini! (cover: assets/dorami-santorini.png)

- Where to find the Dorami workflow:
  - The up-to-date workflow (generation steps, face/outfit consistency rules, and exact commands) is saved at: site/dorami/WORKFLOW.md

- Last generated post (auto-updated):
  - date: 2026-02-14
  - title: Dorami in Munich
  - slug: 2026-02-14-dorami-munich
  - cover: site/dorami/assets/assets/dorami-munich.png


## Azure Image Gen skill





- Skill path: /home/thomas/.openclaw/workspace/skills/azure-image-gen
- Credentials stored in: /home/thomas/.env.azure (secrets NOT stored in memory)
- Preferred default prompt: "photorealistic fluffy kitten, soft lighting, high detail"
- Preferred default size: 1024x1024
- Preferred default format: png
