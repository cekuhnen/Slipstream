<img width="2094" height="745" alt="slipstream" src="https://github.com/user-attachments/assets/ac247895-a140-4624-a46f-84fbfa722ad3" />


# Slipstream

A browser wind tunnel for quick aerodynamic studies of products and vehicles: cars, helmets, scooters, drones, or your own STL/OBJ models. The flow simulation runs on the computer's graphics card (WebGPU). Everything it needs is in this folder, so it works offline and needs no installation.

## Start

Open **index.html** in your browser and pick a tool, or open a tool directly:

| File | What it is |
|---|---|
| `slipstream-3d-tunnel.html` | 3D wind tunnel. Opens in **Design mode** (for students). |
| `slipstream-3d-tunnel.html#engineering` | The same tunnel in **Engineering mode**. |
| `slipstream-profile-lab.html` | 2D sections: wings, spoilers, tubes, blade sections, with angle sweeps. |

Keep the folder together: the tools load their libraries and fonts from `lib/`.

## Browsers

- **Windows:** current Chrome or Edge.
- **Mac:** current Chrome, Edge, or Safari 26 or later. Apple Silicon Macs get the fast 16-bit mode.
- **Linux:** current Chrome. On some Linux systems WebGPU is still switched off by default; if the start page says WebGPU is missing, open `chrome://flags`, enable **Unsafe WebGPU Support** (and, if offered, **Vulkan**), then restart Chrome.

The start page checks the browser and says whether the tunnel will run.

## Sharing with students

Pick one:

1. **Copy the folder** (or the ZIP) to their computers and have them open `index.html`.
2. **Host the folder** on a school web server, course platform or GitHub Pages and send the address of `index.html`.

No accounts are needed. Saved images, ZIPs and PDFs go to the browser's normal Downloads folder.

## Using the 3D tunnel

1. Pick a model or load an STL/OBJ. For files, use **Up axis** and **Flip facing** if the model comes in turned.
2. Set air speed, scale in tunnel, height, yaw and pitch.
3. Wait for the **Stable** indicator in the corner of the view. It needs about two passes of air: the wake behind the model takes one pass to form before the tunnel starts judging it.
4. For still images, switch **Flow view** to **Averaged** and wait until the note says at least one pass.
5. Save:
   - **Save image** – the current view as a 4K PNG, or with **Layered Photoshop file** ticked, a ZIP with a PSD (background, air flow, model, air flow in front of the model, legend) plus each layer as PNG.
   - **Save view set (ZIP)** – 3D, side, top and front. Turn on **Ortho** for true-scale elevations.
   - **PDF report** (Engineering mode) – views, sections, baseline comparison, and rough values with the validation error band.

## How far to trust the numbers

The flow pictures are the most trustworthy result. Absolute drag values are rough: in validation against standard shapes (24 Oct 2026, Ultra resolution), simple bodies read between about 25% low and 18% high, and vehicle-like shapes read about two to three times too high. The solver itself checks out within about 4% where the grid can resolve the flow (sphere at Re 200), and 16-bit mode gives the same results as 32-bit. Use Engineering mode's **baseline comparison** to judge design changes: drag readings carry a ± (how much the average still wobbles), and a difference is reported once it is larger than the combined ± of both runs and at least 3%. **Precise** averaging (Resolution panel) keeps averaging for at least two passes of air (up to four for a restless wake) and narrows the ±. **Surface layer** (Engineering mode, Resolution panel): *Resolved* is the recommended setting and runs at a scaled-down Reynolds number. *Wall model* is experimental: it runs at the real Reynolds number and sets the viscosity of the cells at the surface from the law of the wall. In validation (25 Oct 2026) it improved rounded shapes at moderate Reynolds numbers (sphere at Re 5,000 from 14% to 5% high), read the plate and cube lower, and did not fix vehicle drag (Ahmed body still about three times the published value). *Near-wall damping* turns the turbulence model down right at the surface. Neither setting changes the speed. Why vehicles read high: at this grid size the air layer along the body stays several cells thick and slow, which adds friction and takes away the suction a rounded nose should produce. The pressure behind the body (base pressure) does match published values. Engineering mode includes a **Run validation** button that re-checks the solver on your own computer and produces a validation report.

## What's inside

- Simulation: Lattice Boltzmann method (D3Q19) with regularized collision, a light Smagorinsky turbulence model and curved-wall boundaries, running as WebGPU compute shaders; 16-bit storage where the GPU supports it.
- `lib/` – three.js r128 (3D view), jsPDF 2.5.1 (PDF), JSZip 3.10.1 (ZIP), ag-psd 31.0.2 (Photoshop files), and the Barlow, Barlow Condensed and IBM Plex Mono fonts.
- `licenses/` – the licenses of those libraries (MIT; JSZip is dual MIT/GPL and is used under MIT) and fonts (SIL Open Font License).

## Troubleshooting

- **Black view or "no WebGPU" message:** update the browser and graphics driver; on Linux see Browsers above.
- **"The GPU stopped the simulation":** reload and choose a lower resolution.
- **Slow on a laptop:** use Fast detail (Design mode) or Low/Medium resolution (Engineering mode).
- **Exports do nothing:** check that the browser isn't blocking downloads from local files, or host the folder on a web server.

## License

Slipstream is released under the MIT License. Copyright (c) 2026 Claas Kuhnen (info@ckbrd.de). See `LICENSE`.

You may use, copy, change, share and sell it, as long as the copyright notice stays with it. It comes with no warranty. The results are meant for design exploration and comparison. They are not certified engineering numbers (see the validation notes above for how far off absolute values can be, especially for vehicles).

The bundled libraries (three.js, jsPDF, JSZip, ag-psd) and fonts (Barlow, Barlow Condensed, IBM Plex Mono) keep their own licenses, which are in the `licenses/` folder.
