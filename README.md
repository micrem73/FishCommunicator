# FishCommunicator

**A real-time bridge between a live underwater webcam and an interactive machine-learning model, built for the performance [*Metabolo*](https://sineglossa.it/progetti/metabolo/) by Valerie Tameu.**

FishCommunicator watches a live YouTube fish-cam, calculate a change detection mask of 100 frame areas, and sends that stream over OSC to [Wekinator](http://www.wekinator.org/) so a trained model can turn "what the fish are doing" into control data for sound and performance.

🎥 Demo: https://youtu.be/pX47ZwIquLE

---

## What it does

*Metabolo* stages a "mutual influence" between a human body on stage, an AI, and a marine ecosystem: a live underwater webcam feeds video of a fish community to a machine-learning model, which in turn shapes the soundtrack a dancer performs to. FishCommunicator is the fish-to-music piece of software that makes that link possible.

Pipeline:

1. **Ingest the live feed** — a YouTube livestream is embedded directly in the composition (via an HTML `<iframe>`) and captured as live video, frame by frame. A local video file can be swapped in as an offline fallback/rehearsal source.
2. **Extract visual features** — each frame is pixellated, cropped, and sampled for color and brightness (average color, lightest/darkest points, and a set of 100 sampled values across the frame), reducing the image to a compact numeric fingerprint of the scene.
3. **Stream to Wekinator over OSC** — those numbers are packed into a `/wek/inputs` OSC message (float32) and sent over UDP to port `6448`, where Wekinator's model interprets them in real time.
4. **Train interactively** — the UI includes dedicated buttons ("Send #1 Training OSC", "Send #2 Training OSC", "Send Black OSC") so an operator can label and send example frames to Wekinator during training sessions, without leaving the composition.
5. **Feed the performance** — Wekinator's real-time output is what ultimately drives the soundtrack the performer dances to, closing the loop between fish, machine, and body.

## Repository contents

| File | Description |
|---|---|
| `Youtube2OSC_3_0.vuo` | The main Vuo composition — video capture, feature extraction, and OSC output. |

## Requirements

- **macOS** with [Vuo](https://vuo.org/) 2.4.2 or later (composition was last saved in Vuo 2.4.4)
- **[Wekinator](http://www.wekinator.org/)** (or any OSC-capable receiver) listening on UDP port `6448`, address `/wek/inputs`
- An internet connection, to load the embedded YouTube stream
- *(Optional)* A fallback video at `~/Library/Application Support/Youtube2OSC/pesci_low.m4v` for offline use

## Getting started

1. Install [Vuo](https://vuo.org/download) and open `Youtube2OSC_3_0.vuo`.
2. Launch Wekinator and set it to listen for OSC input on port `6448`.
3. Run the composition — it will load the live feed and begin sending feature data to Wekinator automatically.
4. Train a Wekinator model so that it outputs a number of parameters intended to smooth or trigger audio effects.
5. Route Wekinator's output into your sound engine / performance patch of choice (i.e. Ableton Live), matching the output to the sound engine effects of tracks.

## Credits

FishCommunicator was developed by **Michele Cremaschi** as the technical/creative development for **Metabolo**, an artistic project by **Valerie Tameu**, curated and produced by **Sineglossa** within the [*Food Data Digestion*](https://sineglossa.it/progetti/food-data-digestion/) research programme (with Play with Food and the Free University of Bozen-Bolzano, supported by Fondazione Compagnia di San Paolo).

*Metabolo* has been presented at Sineglossa Creative Ground (Ancona), Lavanderia a Vapore (Turin), and Ars Electronica Festival 2023 (Linz, Austria). Full credits and documentation: https://sineglossa.it/progetti/metabolo/
