# Arduino Music Visualization

## Goal

Create an Arduino-based music visualizer using the Fast Fourier Transform (FFT). The project will consist of two components: **code** and **hardware**.

### Code

The code for this project will use an FFT library to do a frequency analysis of an audio signal that is read in via one of the analog pins on the Arduino. Once the frequency spectrum is obtained, the code will allow the user to easily manipulate external hardware (e.g. LEDs) to respond to the various frequencies in the audio signal. There are multiple analysis techniques that can be applied to the frequency spectrum:

#### Binning

In order for a continuous range of frequencies to be displayed visually with a discrete number of components, the frequencies must be broken up into **bins**, with each bin containing a range of frequencies. The strategy for dividing the frequencies will likely require research and trial-and-error to determine what frequencies are most common in modern music and what they represent.

#### Amplitude Adjustments

Depending on the physical visualization method, it may be important to determine the **amplitude** of the frequencies within each bin. For example, a visualization may have bars that move up depending on the amplitude of certain frequencies:

![visualizer_bars](http://blog.motionisland.com/wp-content/uploads/2018/04/after-effects-audio-spectrum-with-color-bar.gif)

This effect requires the code to determine the amplitude within a bin and (potentially) scale the amplitude readings based on the input voltage, which will change based on the volume of the audio source.
