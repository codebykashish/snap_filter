# AR Fashion — MindAR.js Body Tracking

Real body tracking AR experience. Scan QR → camera opens → 3D design follows your body.

## Setup in 5 steps

### 1. Add your 3D model
Put your `.glb` file inside the `assets/` folder and rename it `model.glb`

(Or change the filename in `index.html` line: `loader.load('assets/model.glb', ...`)

### 2. Test locally
You MUST use HTTPS for camera access. Easiest way:

```bash
# Install a local HTTPS server
npm install -g local-ssl-proxy serve

# In one terminal — serve the files
serve . -p 8080

# In another terminal — add HTTPS on top
local-ssl-proxy --source 8443 --target 8080
```

Then open: `https://localhost:8443` on your computer
Or use VS Code Live Server extension with HTTPS enabled.

**Easiest option:** Push to Vercel (step 4) and test directly on your phone — no local HTTPS setup needed.

### 3. Push to GitHub
```bash
git init
git add .
git commit -m "first ar build"
git remote add origin https://github.com/YOURUSERNAME/ar-fashion.git
git push -u origin main
```

### 4. Deploy to Vercel
1. Go to vercel.com → New Project
2. Import your GitHub repo
3. Click Deploy — done
4. Vercel gives you a URL like `ar-fashion.vercel.app`

### 5. Generate QR code
1. Go to https://qr-code-styling.com
2. Enter your Vercel URL
3. Download the QR code image
4. Scan it with your phone — the AR opens

---

## Adjusting the model

All adjustments are in `index.html`. Find this section:

```javascript
model.scale.set(2, 2, 2);        // Make bigger = higher number
model.position.set(0, -0.3, 0);  // Move up/down = change Y value
```

**Scale guide:**
- Too small → increase from 2 to 3 or 4
- Too large → decrease to 1 or 1.5

**Position guide (Y axis):**
- Model too high on body → make Y more negative (e.g. -0.5, -0.8)
- Model too low → make Y less negative (e.g. -0.1, 0)

---

## Body anchor points

The model is currently anchored to point 11 (left shoulder).
Change `mindarThree.addAnchor(11)` to any number below:

```
0  = nose
1  = left eye inner
2  = left eye
3  = left eye outer
4  = right eye inner
5  = right eye
6  = right eye outer
7  = left ear
8  = right ear
9  = mouth left
10 = mouth right
11 = LEFT SHOULDER  ← currently using this (good for torso designs)
12 = right shoulder
13 = left elbow
14 = right elbow
15 = left wrist
16 = right wrist
17 = left pinky
18 = right pinky
19 = left index
20 = right index
21 = left thumb
22 = right thumb
23 = left hip
24 = right hip
25 = left knee
26 = right knee
27 = left ankle
28 = right ankle
```

**For a dress/torso design:** anchor 11 or 12 (shoulders) works best
**For a full outfit:** try anchoring to 23 or 24 (hips) and scaling up

---

## File structure

```
ar-fashion/
├── index.html       ← main AR page (edit this)
├── assets/
│   └── model.glb    ← your 3D model goes here
└── README.md
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| Black screen | Make sure you're on HTTPS (Vercel URL, not localhost) |
| Camera permission denied | Refresh page and tap Allow |
| Model not visible | Check browser console for errors — model file missing? |
| Model wrong size | Adjust `model.scale.set()` values |
| Model in wrong position | Adjust `model.position.set()` Y value |
| Tracking lost | Make sure full body is visible, good lighting, step back |
| Slow / laggy | Model file too large — compress .glb to under 5MB |