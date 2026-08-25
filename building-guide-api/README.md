# Building Guide API

Static JSON + photos for a new app. Hosted with **GitHub Pages**.

Categories: House, Statue, Vehicle, Beacon, Redstone.

## Live endpoints

After Pages is on (main branch, folder `/`):

```
https://amonnoris1.github.io/building-guide-api/v1/index.json
https://amonnoris1.github.io/building-guide-api/v1/categories.json
https://amonnoris1.github.io/building-guide-api/v1/builds.json
https://amonnoris1.github.io/building-guide-api/v1/builds/6.json
https://amonnoris1.github.io/building-guide-api/v1/media/builds/covers/{file}.webp
https://amonnoris1.github.io/building-guide-api/v1/media/builds/steps/{file}.webp
https://amonnoris1.github.io/building-guide-api/v1/media/materials/{file}.webp
```

GitHub Pages is HTTPS and sends `Access-Control-Allow-Origin: *`, so a new iOS / Android / web app can `GET` these files.

## What the new app should call

| Endpoint | Use |
|---|---|
| `GET /v1/index.json` | Totals |
| `GET /v1/categories.json` | Category list + counts |
| `GET /v1/builds.json` | All 264 builds (summary: cover, dimensions, description, style) |
| `GET /v1/builds/{id}.json` | One full build: materials, step photos, showcase |
| `GET /v1/media/...` | Cover / step / material images |

Do **not** use `raw.githubusercontent.com` for the app. Use the `github.io` Pages URLs.

## Publish this folder

This `api/` folder is the GitHub repo root (so `/v1/...` is the public path).

1. On GitHub, create a **public** repo named `building-guide-api`.
2. From this folder:

```bash
cd "/Users/mac/Documents/SwiftProjects/Building Guide Build/api"
git init
git add .
git commit -m "Add Building Guide catalog API"
git branch -M main
git remote add origin https://github.com/amonnoris1/building-guide-api.git
git push -u origin main
```

3. Repo → **Settings → Pages**
   - Source: **Deploy from a branch**
   - Branch: `main` / `/` (root)
4. Wait a minute, then:

```bash
curl https://amonnoris1.github.io/building-guide-api/v1/categories.json
```

`media/` is about **2.6 GB**. The first push is slow. GitHub warns on repos over ~1 GB; files here are small `.webp`s, so it usually still works. If GitHub blocks the push, keep JSON on GitHub and host `v1/media/` on another HTTPS host, then rebuild with `--public-base`.

## Rebuild / change host

From the app project root:

```bash
# harvest again
python3 scripts/build_our_api.py --public-base https://amonnoris1.github.io/building-guide-api --path-prefix /v1

# only rewrite URLs in existing JSON
python3 scripts/build_our_api.py --retarget-only --public-base https://amonnoris1.github.io/building-guide-api --path-prefix /v1
```

If the GitHub username or repo name is different, pass that Pages origin as `--public-base`.
