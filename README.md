# FM Receiver Flowgraph

## Project Description

This project implements an FM radio receiver using an RTL-SDR device. It allows you to select from preset stations, demodulates the FM broadcast into audio, records the audio to a WAV file, and provides real-time visualizations through time, frequency, and waterfall displays.

## Overview

The flowgraph is designed to:
- **Receive** live FM broadcasts via an RTL-SDR device.
- **Demodulate** the FM signal into an audio stream.
- **Record** the demodulated audio to a WAV file.
- **Visualize** the received signal with three different displays:
  - **Time Sink:** Shows the time-domain waveform.
  - **Frequency Sink:** Displays the static frequency spectrum (FFT).
  - **Waterfall Sink:** Provides a spectrogram view that tracks signal changes over time.
- **Select Stations:**  
  A station chooser offers three preset options:
  - Galei Tzahal 
  - Radio Lev HaMedina
  - Radio Kol Chai 

## Flowgraph Structure

### Variables
- **samp_rate:** Set to 2,048,000 Hz. This is the rate at which the RTL-SDR source acquires samples.
- **freq:** Holds the current center frequency, updated via the station chooser.

### Blocks

1. **Station Chooser (QT GUI Chooser):**  
   Provides a drop-down list with three preset stations. When a station is selected, the `freq` variable is updated with the corresponding frequency, retuning the RTL-SDR source.

2. **Soapy RTL-SDR Source:**  
   Interfaces with the RTL-SDR hardware using SoapySDR. It receives raw complex samples at the defined sample rate and tunes to the frequency specified by `freq`.

3. **Rational Resampler:**  
   Converts the high sample rate from the RTL-SDR source to a lower rate (192,000 Hz) suitable for FM demodulation.

4. **Analog WFM Receiver:**  
   Demodulates the FM signal from the resampled data, producing an audio stream at 48,000 Hz after further decimation.

5. **Audio Sink:**  
   Plays the demodulated audio through the speakers for live listening.

6. **Wav File Sink:**  
   Records the demodulated audio stream into a WAV file (e.g., `recorded_audio.wav`) for later playback or analysis.

7. **Visualization Blocks:**  
   - **QT GUI Time Sink:** Displays the time-domain waveform of the received signal.
   - **QT GUI Frequency Sink:** Shows the frequency-domain spectrum, revealing the energy distribution across frequencies.
   - **QT GUI Waterfall Sink:** Provides a spectrogram view (waterfall) where frequency is on the X-axis, time on the Y-axis, and color intensity represents signal strength.

## Data Flow Summary

1. **Signal Acquisition:**  
   The RTL-SDR Source captures the RF signal at 2,048,000 Hz, tuned to the current frequency (`freq`).

2. **Station Selection:**  
   The station chooser updates the `freq` variable based on the selected preset, retuning the source accordingly.

3. **Visualization:**  
   The raw signal is split and sent to the Time, Frequency, and Waterfall Sinks, providing multiple real-time views of the signal.

4. **Processing Chain:**  
   The signal is resampled and passed to the Analog WFM Receiver, which demodulates it into an audio stream at 48,000 Hz.

5. **Output:**  
   - **Audio Sink:** Plays the live audio.
   - **Wav File Sink:** Records the audio stream to a file.

## Conclusion

This FM Receiver Flowgraph demonstrates the versatility of software-defined radio using GNU Radio. It integrates live audio playback, recording, and multiple real-time visualizations with an interactive station chooser, making it an effective tool for exploring FM broadcasts and SDR technology.
