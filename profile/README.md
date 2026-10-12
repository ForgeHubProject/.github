<h1 align="center">ForgeHub</h1>

<p align="center">
  <b>Version control that understands what you're actually making.</b><br>
  Reviewable, mergeable, traceable changes for 3D models, hardware designs and every other file Git only sees as "binary".
</p>

---

## Why we're building this

Git is wonderful at text. Hand it a `.glb`, a `.blend` or a CAD export and all it can say is *"Binary files differ."* So hardware and 3D teams fall back on screenshots, chat threads and review meetings, and weeks later nobody can answer the simple questions: **what changed, why, and who signed off?**

ForgeHub is our answer. Keep Git underneath for storage and transport, and teach the layer above it what a file *is*, so a diff shows a part that moved, a material that changed or a node that was added, instead of a wall of bytes.

## The three pieces

Everything here fits together as one pipeline. Each repo is useful on its own.

```
   FHR  ───────────────▶  forge  ───────────────▶  ForgeHub
 handler registry       the command line        the web platform
 (what a format is)     (diff + merge locally)  (review it together)
```

| | Repo | What it is |
|---|---|---|
| 🌐 | **[ForgeHub](https://github.com/ForgeHubProject/ForgeHub)** | The collaboration platform: repositories, pull requests, issues and review in the browser, with visual 3D diffs, plus Git over HTTPS and SSH. Fastify + React + SQLite/Postgres/MySQL. |
| 🛠️ | **[forge](https://github.com/ForgeHubProject/forge)** | The command-line tool. Fully Git-compatible, but its `diff` and `merge` are format-aware: `forge diff model.glb` tells you what changed inside the model. Written in Go. |
| 📦 | **[FHR](https://github.com/ForgeHubProject/FHR)** | The Forge Handler Repository. The official registry of *format handlers*: how a format is diffed, merged, converted and rendered. Home of the shared contract between forge and ForgeHub. |

## What's supported today

FHR ships handlers for a growing list of formats, 3D first:

- **3D scenes and meshes:** glTF/GLB, OBJ, STL, PLY, 3MF, OFF, Collada, X3D, VRML, FBX, USD, 3DS, AMF, DXF and Blender `.blend`
- **Data and documents:** CSV, JSON, TOML, GeoJSON, Jupyter notebooks
- **Images and media:** PNG, JPEG, GIF, SVG, WAV

Plain text always works through the built-in line-level diff, and any 3D format can be converted to another through a glTF pivot (`forge convert`).

## How it works, in one minute

1. **A handler describes a format.** It answers four questions: does this file match, what changed between two versions, how do two edits merge, and how should it look.
2. **forge runs handlers locally** and speaks plain Git to any remote, so nothing about your storage or hosting has to change.
3. **ForgeHub renders the result** in the browser: side-by-side and overlay 3D views, a change queue, comments and reviews, on top of real bare Git repositories.
4. Everyone speaks one shared language, a `StructuredDiff`, so a new format only has to be taught once.

## Get started

**Try the CLI**

```bash
curl -fsSL https://raw.githubusercontent.com/forgehubproject/forge/main/install.sh | bash
forge diff model.glb
```

**Run the platform**

```bash
git clone https://github.com/ForgeHubProject/ForgeHub.git
cd ForgeHub
cp .env.example .env        # set JWT_SECRET
docker compose up -d --build
```

Then open `http://localhost:8080`. Full setup, configuration and the Git-over-SSH guide are in the [ForgeHub README](https://github.com/ForgeHubProject/ForgeHub#readme).

## Add a format

Missing something you work with? Handlers are meant to be written by the people who need them. Start from the skeletons in [FHR](https://github.com/ForgeHubProject/FHR): a TypeScript handler for the quick path, or a native one in any language that talks JSON over stdin/stdout. The renderer is always TypeScript and React, so your format gets a proper view in ForgeHub too.

## Join in

- **Found a bug or have an idea?** Open an issue on the relevant repo: [ForgeHub](https://github.com/ForgeHubProject/ForgeHub/issues) · [forge](https://github.com/ForgeHubProject/forge/issues) · [FHR](https://github.com/ForgeHubProject/FHR/issues)
- **Want to contribute?** Pull requests are welcome. Each repo's CI must pass, and `main` is protected, so changes land through review.
- **New format handlers** are the most valuable contribution; start in [FHR](https://github.com/ForgeHubProject/FHR).

ForgeHub and forge are MIT licensed.

<p align="center"><sub>Built so that "what changed?" always has an answer.</sub></p>
