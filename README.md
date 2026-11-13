# CRT Oscilloscope

A standalone, browser-based CRT oscilloscope simulator for stereo audio and oscilloscope music.

The project renders audio-driven X-Y and Y-T traces using Web Audio API and WebGL2. It focuses on preserving the source signal's geometry while simulating the visible behaviour of an electron beam and phosphor screen.

## Demo

**CRT Oscilloscope playing _Primer_ by DJ_Level_3 and Marv1994 / BUS ERROR Collective:**

[![Watch the CRT Oscilloscope demonstration](https://img.youtube.com/vi/x7545AS1tTQ/maxresdefault.jpg)](https://youtu.be/x7545AS1tTQ)

This is an independent recording of the composition being played through CRT Oscilloscope. It is not the original _Primer_ production video.

## Features

- **X-Y mode:** stereo left/right channels drive horizontal and vertical beam deflection.
- **Y-T mode:** displays the audio waveform over time.
- **Audio file playback:** load a local audio file in the browser.
- **Live microphone input:** use the microphone as a signal source (browser permission required).
- **WebGL2 rendering:** instanced beam-segment rendering with an accumulated phosphor-energy buffer.
- **Source-rate-aware analysis:** detects the source sample rate for supported containers and requests a matching audio context where the browser allows it.
- **Beam controls:** scale, beam current/intensity, focus and synchronization offset.
- **Z-axis blanking simulation:** estimates rapid transitions from the XY trajectory and attenuates the beam during those transitions. This is a heuristic; the stereo input does not contain a separate Z/blanking channel.
- **Phosphor persistence profiles:** fast, standard, dual and long-tail presets using a three-component decay model.
- **Gaussian glass glow:** separate downsample and blur passes, independent of the sharp beam image.
- **Trajectory modes:** RAW and Catmull-Rom interpolation.
- **Quality controls:** adjustable rendering quality for different devices.
- **Bilingual interface:** English and Russian UI, with the selected language remembered in the browser.

## Getting started

1. Download `index.html` from this repository.
2. Open it in a modern browser with WebGL2 and Web Audio API support.
3. Load a local audio file and press **Play**.
4. Switch between **X-Y** and **Y-T** modes and adjust the controls.

No installation, build step, package manager or external runtime is required.

The microphone input requires browser permission and a secure context (HTTPS or localhost). Audio decoding depends on the codecs supported by the browser.

## Public web version (GitHub Pages)

The repository includes a GitHub Actions workflow for publishing the app as a public website.

The application is published from the `main` branch using GitHub Actions. The latest deployment completed successfully.

Open the interactive app at:

`https://ancientsus.github.io/CRT-Oscilloscope/`

The app is static and client-side: audio files selected in the page are decoded locally in the visitor's browser; they are not uploaded to a project server. Microphone input also stays in the browser and requires explicit permission.

## Controls

| Control | Purpose |
| --- | --- |
| Language | Switch the interface between English (EN) and Russian (RU) |
| File | Load an audio file from the device |
| Microphone | Use live microphone input |
| X-Y / Y-T | Switch display mode |
| Position | Seek within the loaded track |
| Volume | Playback volume |
| Synchronization | Adjust image timing relative to audio |
| Scale (V/DIV) | Change signal scale |
| Beam Current | Change the simulated beam energy/intensity |
| Focus | Change beam thickness |
| Z-axis blanking | Set how strongly fast transitions are suppressed |
| Transition threshold | Adjust the threshold used to detect rapid transitions |
| Persistence | Set the base phosphor decay time |
| Phosphor profile | Select Fast, Standard, Dual or Long persistence |
| Gaussian glow | Adjust the soft halo around the beam |
| Time/DIV | Adjust the visible sample window in Y-T mode |
| Auto brightness | Enable automatic beam-energy compensation |
| Quality | Switch rendering quality |
| Trajectory | Switch between RAW and Catmull-Rom paths |

## Technical overview

CRT Oscilloscope is implemented as a single HTML document containing its CSS, JavaScript and GLSL shaders.

- **Input and playback:** Web Audio API.
- **Signal analysis:** decoded audio channel samples, with source sample-rate detection where supported.
- **Rendering:** WebGL2 and GLSL ES 3.00.
- **Beam geometry:** instanced line-segment quads with a Gaussian energy distribution.
- **Phosphor simulation:** accumulated energy in a multi-component texture with exponential decay.
- **Glass glow:** a separate downsample + separable Gaussian blur pipeline.
- **GPU timing:** asynchronous timer queries when the browser exposes the relevant WebGL extension.

The Z-axis blanking feature is an approximation based on trajectory speed and direction changes. It is not a reconstruction of a physical oscilloscope's separate Z input.

## Credits

The demonstration uses **_Primer_**, created by **DJ_Level_3 and Marv1994** as part of the **BUS ERROR Collective**.

- [Original production and credits on Demozoo](https://demozoo.org/productions/371249/)
- [Original production video](https://www.youtube.com/watch?v=6DifrkaALOg)
- [DJ_Level_3's Bandcamp release](https://djlevel3.bandcamp.com/album/1-year-of-bec)

The video linked above is a separate recording made by the CRT Oscilloscope project author. The original music and production remain the work of their respective creators. CRT Oscilloscope is an independent project and is not affiliated with BUS ERROR Collective.

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for the full license text.

The MIT License applies to the CRT Oscilloscope source code. It does not grant rights to third-party works featured in demonstrations, including _Primer_, its music, or its original visuals. Those remain the property of their respective rights holders.
