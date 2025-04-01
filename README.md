#FM Receiver Flowgraph – README
Overview
This flowgraph demonstrates an FM radio receiver using an RTL-SDR device. The design allows you to:

Receive live FM broadcasts using the RTL-SDR hardware.

Demodulate the FM signal into an audio stream.

Record the demodulated audio to a WAV file.

Visualize the signal in real time using time, frequency, and waterfall displays.

Select Stations from three preset options:

Galei Tzahal (98.8 FM)

Radio Lev HaMedina

Radio Kol Chai (93.0 FM)

Flowgraph Structure
Variables
samp_rate (2,048,000 Hz):
Defines the sample rate at which the RTL-SDR source acquires data.

freq:
Holds the current center frequency, updated by the station chooser to retune the RTL-SDR source.

Blocks
Station Chooser (QT GUI Chooser):
Provides a drop-down list with three preset stations. When a station is selected, the chooser updates the freq variable with the corresponding frequency (e.g., 98.8 MHz for Galei Tzahal), retuning the RTL-SDR source accordingly.

Soapy RTL-SDR Source:
Uses SoapySDR to interface with the RTL-SDR hardware. It receives raw complex samples at 2,048,000 Hz and is tuned to the frequency specified by freq.

Rational Resampler:
Converts the high sample rate from the RTL-SDR source to a lower rate (192,000 Hz) that is compatible with the FM demodulation process.

Analog WFM Receiver:
Demodulates the FM signal from the resampled data. It decimates the rate further to produce an audio stream at 48,000 Hz.

Audio Sink:
Plays the demodulated audio live through the speakers, allowing you to listen to the FM broadcast in real time.

Wav File Sink:
Records the demodulated audio stream into a WAV file (e.g., recorded_audio.wav). This block allows you to capture the audio for later playback or analysis.

Visualization Blocks:

QT GUI Time Sink:
Displays the time-domain waveform of the received signal, showing amplitude variations over time.

QT GUI Frequency Sink:
Shows a real-time static spectrum (FFT) view of the signal, highlighting how the signal’s energy is distributed across frequencies.

QT GUI Waterfall Sink:
Provides a spectrogram (waterfall) view, where frequency is plotted on the X-axis, time on the Y-axis, and color intensity represents signal strength. This view helps you see how the spectrum evolves over time.

Data Flow Summary
Signal Acquisition:
The RTL-SDR source captures the radio frequency signal at 2,048,000 Hz, tuned to the frequency determined by the station chooser.

Station Selection:
The station chooser updates the freq variable, which retunes the RTL-SDR source to one of the preset stations.

Visualization:
The raw signal is split and sent to the Time, Frequency, and Waterfall Sinks, providing multiple real-time visual perspectives.

Processing Chain:
The signal passes through the Rational Resampler and then the Analog WFM Receiver to be demodulated into an audio stream at 48,000 Hz.

Output:

Audio Sink: Plays the audio live.

Wav File Sink: Records the audio into a WAV file for later use.

Conclusion
This flowgraph combines live FM reception with interactive station selection, comprehensive real-time visualizations, and audio recording capabilities. By using preset stations via the chooser, you can easily switch between broadcasts. The audio is both played live and recorded, while the Time, Frequency, and Waterfall Sinks provide detailed visual feedback of the signal’s characteristics. This design effectively demonstrates the power and versatility of software-defined radio using GNU Radio
