# FP1 / FP3 Dental Articulation Viewer

Single-file, Carrd-compatible 3D viewer that loads the four GLB files from
`https://exocadelite.github.io/TOOTH-LIBRARY/` and shows **FP1 and FP3 side
by side** with independent Upper / Lower / Both toggles per side.

## Files

| File | When to use |
|---|---|
| `embed.html` | Paste the entire contents into a **Carrd → Embed** element. |
| `index.html` | Drop into a GitHub Pages repo and serve as a full page. Iframe it into Carrd if Carrd ever blocks ES module imports. |

Both files share the same script — keep them in sync.

## How it satisfies the spec

- **Two Three.js scenes**, one per side (FP1, FP3). Each side loads its `upper.glb` and `lower.glb` into the **same scene** for true articulation.
- **No model transforms.** `position`, `rotation`, `scale` are never touched. Only `object.visible` flips when a button is clicked.
- **Camera framing** uses the combined group's bounding box only to position the camera + OrbitControls target — the models themselves are untouched.
- **Default = Both** for both sides.
- **Button isolation:** FP1 buttons only affect FP1; FP3 buttons only affect FP3.
- **Enamel material:** linear baseColor `[0.78, 0.60, 0.05]`, metalness `0.0`, roughness `0.38` — applied via `MeshStandardMaterial`.
- **Lighting:** moderate ambient + directional key + directional fill, exposure `0.85`, neutral tone mapping.
- **Transparent canvas background** + `drop-shadow(0 24px 28px rgba(0,0,0,0.55))` on the stage wrapper.
- **Error states:** if any GLB fails to load, a clear status line names the file (e.g. `FP1 UPPER failed to load. Check GitHub filename.`).

## Using it in Carrd

1. Carrd dashboard → add an **Embed** element.
2. Style: **Code**.
3. Paste the entire contents of `embed.html`.
4. Publish.

If Carrd refuses ES-module imports on your plan:

1. Push `index.html` to a GitHub repo with Pages enabled (e.g. `dental-viewer`).
2. In Carrd, add an **Embed** with this iframe:

   ```html
   <iframe
     src="https://<your-user>.github.io/dental-viewer/"
     style="width:100%;height:780px;border:0;"
     allow="xr-spatial-tracking"
     loading="lazy">
   </iframe>
   ```

## Verified URLs

All four GLB files return `HTTP 200` with `content-type: model/gltf-binary` and `access-control-allow-origin: *`:

- `https://exocadelite.github.io/TOOTH-LIBRARY/Upper%20FP1.glb` (5.7 MB)
- `https://exocadelite.github.io/TOOTH-LIBRARY/lower%20FP1.glb` (5.0 MB)  ← note lowercase `lower`
- `https://exocadelite.github.io/TOOTH-LIBRARY/Upper%20FP3.glb` (7.0 MB)
- `https://exocadelite.github.io/TOOTH-LIBRARY/Lower%20FP3.glb` (6.6 MB)

CORS is open, so the viewer works from any origin (Carrd, your own domain, localhost).

## Local test

```bash
cd tooth-library-carrd
python3 -m http.server 5180
# then open http://localhost:5180/index.html
```
