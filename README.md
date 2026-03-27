# Blurrier - Fourier Seek

<img src="blurrier.gif?raw=true">

![with-coffee](https://img.shields.io/badge/made%20with-%E2%98%95%EF%B8%8F%20coffee-yellow.svg)
![with-water](https://img.shields.io/badge/made%20with-%F0%9F%92%A7%20water-blue.svg)
![with-love](https://img.shields.io/badge/made%20with-%F0%9F%92%8C-red.svg)

[Live Demo](https://apidcloud.github.io/blurrier/)

Decode sparse video keyframes, reconstruct smooth seek previews with 2D FFT spatial blur and 1D FFT temporal interpolation, entirely on the client-side. Includes both an MP4 keyframe demo and a WebRTC loopback demo for sparse preview streaming.

## How it works

1. **Local MP4 demo:** [MP4Box](https://github.com/gpac/mp4box.js) demuxes a local MP4, extracts keyframes, and feeds them into the blur + interpolation pipeline.

2. **WebRTC loopback demo:** Encoded keyframes are sent through a local DataChannel loopback, then reconstructed on the receiver with the same blur + interpolation pipeline.

3. **Spatial blur (2D FFT):** Each keyframe's Y-plane (grayscale) is downscaled to a power-of-2 preview size, transformed with a 2D FFT, lowpass-filtered (keeping ~15% of coefficients), and reconstructed via inverse FFT into a blurry but recognizable preview.

4. **Temporal interpolation (1D FFT + Phase Shift Theorem):** Batches of keyframes are transformed along the time axis per-pixel. High-frequency temporal bins are discarded, and synthetic in-between frames are generated at fractional time offsets, producing multiplier x more frames than were actually decoded (default 4x).

The broader target is **live streaming and WebRTC**: send only keyframes, or a sparse subset of encoded chunks, and let the client fill in the gaps with FFT-based interpolation.

## Limitations

Grayscale only, power-of-2 dimensions, and intentionally low-resolution preview output.

## Future work

Color (3x FFTs for YCbCr/RGB channels), denoising, and maybe higher resolution.

## Credits

- Inspired by [kyndinfo](https://x.com/kyndinfo)'s article on [Fourier Series](https://kyndinfo.notion.site/Fourier-Series-caa91c112da043888cbe45f18392caff);
- Using [MP4Box](https://github.com/gpac/mp4box.js) for MP4 demuxing.
