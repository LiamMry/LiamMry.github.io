# Homepage demo clips

These clips are embedded on the homepage by `_includes/pixel_space_demo.html`
(the "Demo 2 — The same mechanism, in pixel space" section of `index.md`).
All three use the same FFHQ score network; only the conditioning differs.

## Files

- `ffhq_unconditional.mp4` — reverse diffusion from pure noise to one sample
  from the FFHQ prior. Single 512×512 face, ~7 s loop.
- `ffhq_posterior_random_inpainting.mp4` — 2×2 grid: the fixed observation y
  (random pixel mask + noise, labelled "y", top-left) and three independent
  posterior samples denoising in parallel. From the calibrated posterior
  sampling work.
- `ffhq_posterior_motion_blur.mp4` — same 2×2 layout with a motion-blurred
  observation. Also from the calibrated posterior sampling work.

All clips: 512×512, ~7.2 s, H.264 `yuv420p`, ~0.6 MB each, autoplay-muted-loop
on the page.

## Replacing or adding a clip

Keep the same format (square, `yuv420p`, even dimensions for Safari,
`-movflags +faststart`, < 1.5 MB). From a directory of frames `frame_%03d.png`:

```bash
ffmpeg -framerate 9 -i frame_%03d.png \
  -vf "tpad=stop_mode=clone:stop_duration=1.5,scale=512:512:flags=neighbor" \
  -c:v libx264 -pix_fmt yuv420p -crf 26 -movflags +faststart out.mp4
```

Sampling every ~20 steps of a 1000-step trajectory and saving the
x̂₀-prediction (not raw x_t) gives the cleanest visual. If a file is missing
or fails to load, the include shows a dashed "Clip unavailable" placeholder
instead of a broken player.
