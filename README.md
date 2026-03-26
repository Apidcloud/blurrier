# Blurrier - Fourier Seek

<img src="blurrier.gif?raw=true">

![with-coffee](https://img.shields.io/badge/made%20with-%E2%98%95%EF%B8%8F%20coffee-yellow.svg)
![with-water](https://img.shields.io/badge/made%20with-%F0%9F%92%A7%20water-blue.svg)
![with-love](https://img.shields.io/badge/made%20with-%F0%9F%92%8C-red.svg)

[Live Demo](https://apidcloud.github.io/blurrier/)

Small experiment to only decode video keyframes, reconstruct smooth scrub previews with 2D FFT spatial blur and 1D FFT temporal interpolation, entirely on the client-side. Built for live streaming and WebRTC.

## How it works

1. **Spatial blur (2D FFT):** Each keyframe's Y-plane (grayscale) is downscaled to a power-of-2 preview size, transformed with a 2D FFT, lowpass-filtered (keeping ~15% of coefficients), and reconstructed via inverse FFT into a blurry but recognizable preview.

2. **Temporal interpolation (1D FFT + Phase Shift Theorem):** Batches of keyframes are transformed along the time axis per-pixel. High-frequency temporal bins are discarded, and synthetic in-between frames are generated at fractional time offsets, producing 4x more frames than were actually decoded.

The current demo uses [MP4Box](https://github.com/gpac/mp4box.js) to demux a local MP4 file and extract only keyframes, simulating a chunked delivery pipeline. The real target is **live streaming and WebRTC**: a server sends only keyframes (or a sparse subset of encoded chunks) and the client fills in the gaps with FFT interpolation.

## Limitations

Grayscale only, and power-of-2 dimensions.

## Future work

Color (3× FFTs for YCbCr/RGB channels), denoising, and maybe higher resolution.

## Credits

- Inspired by [kyndinfo](https://x.com/kyndinfo)'s article on [Fourier Series](https://kyndinfo.notion.site/Fourier-Series-caa91c112da043888cbe45f18392caff);
- Using [MP4Box](https://github.com/gpac/mp4box.js) for MP4 demuxing.
