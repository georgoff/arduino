# Arduino Music Visualization

## Goal

Create an Arduino-based music visualizer using the Fast Fourier Transform (FFT). The project will consist of two components: **code** and **hardware**.

### Code

The code for this project will use an FFT library to do a frequency analysis of an audio signal that is read in via one of the analog pins on the Arduino. Once the frequency spectrum is obtained, the code will allow the user to easily manipulate external hardware (e.g. LEDs) to respond to the various frequencies in the audio signal. There are multiple analysis techniques that can be applied to the frequency spectrum:

#### Binning

In order for a continuous range of frequencies to be displayed visually with a discrete number of components, the frequencies must be broken up into **bins**, with each bin containing a range of frequencies. The strategy for dividing the frequencies will likely require research and trial-and-error to determine which frequencies are most common in modern music and what they represent.

#### Amplitude Adjustments

Depending on the physical visualization method, it may be important to determine the **amplitude** of the frequencies within each bin. For example, a visualization may have bars that move up depending on the amplitude of certain frequencies:

![visualizer_bars](http://blog.motionisland.com/wp-content/uploads/2018/04/after-effects-audio-spectrum-with-color-bar.gif)

This effect requires the code to determine the amplitude within a bin and (potentially) scale the amplitude readings based on the input voltage, which will change based on the volume of the audio source.

### Hardware

#### Custom-Built Arduino Solution

Originally, I planned on building my own circuit that would simply feed an audio signal into the analog input of an Arduino board and then analyze the signal from there using `analogRead()`; however, this technique severely limits the sampling rate that can be achieved because `analogRead()` is slow [give justification here]. This led to me diving into the dizzingly complicated world of [analog-to-digital converters](https://en.wikipedia.org/wiki/Analog-to-digital_converter) (ADCs) and how they affect sampling rates.

##### ADCs and Arduino

Nick Gammon has a [very in-depth article](https://www.gammon.com.au/adc) about the ADC on the Arduino and how it works. There is a lot of complicated information about clock cycles and the timing of analog-to-digital conversion, but what was most important to me was the concept of a **prescaler**. A prescaler, as Nick puts it, "divides down the **processor** clock speed to give an **ADC** clock speed".

##### The Prescaler

By changing the prescaler used by the ADC in the Arduino, it's possible for us to increase the maximum possible sampling rate of our signal. Without changing the prescaler, the maximum sampling rate of a standard Arduino is [around 10 kHz](https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency). Given that humans can hear sounds in the (approximate) range of [20 to 20,000 Hz](https://en.wikipedia.org/wiki/Hearing_range), we need a sampling rate of at least around 40 kHz (this is due to the [Nyquist-Shannon sampling theorem](https://en.wikipedia.org/wiki/Nyquist%E2%80%93Shannon_sampling_theorem)). The prescaler is set to a default value of 128, but it can be lowered to any value that is a power of 2. This leads to the following sampling rates:

| Prescaler | Conversions/sec |
| :---: | :---: |
| 2 | 615,385 |
| 4 | 307,692 |
| 8 | 153,846 |
| 16 | 76,923 |
| 32 | 38,462 |
| 64 | 19,231 |
| 128 | 9,615 |
