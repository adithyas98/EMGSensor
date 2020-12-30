# EMG Measurement Channel Design 

The signal was processed in the way shown in the block diagram below. Each component of this block diagram will be explained subsequently

```mermaid
graph LR
A[Sensor] -->B[Pre-Amplification]
	B -->C[Analog Filtering]
	C-->D[Secondary Amplification]
	D -->E[Arduino Analog Input]
	E -->|ADC|F[Software]
	F -->G[Output]

```

We will not cover the sensor or past the analog-to-digital conversion since this is out of the scope of the design assignment and thus will only cover the analog circuitry required to process the signals received by the sensor.

#### Pre-Amplification:

In this stage of the signal processing circuit, we must amplify the signals received in order to make sure they are detected and processed further along the circuitry. In order to do so the AD620 IC chip was used as an instrumentation amplifier with $\text{Gain} \approx 10$ in order to make sure that signals were properly detected while attempting to not make it difficult to amplify the unnecessary signals that will get filtered out. For this gain, $R_g =5488.889 \approx 5k\Omega $ (Note the actual gain would be 10.88). We choose a a gain this large because the voltage range is on the order of millivolts[^1]

[^1]:https://skeletalmusclejournal.biomedcentral.com/articles/10.1186/s13395-018-0167-9

#### Analog Filtering:

The subsequent step is to filter the signal such that we are only left with the frequencies that carry the information we want to analyze, thereby taking out the very low frequencies (Such as other electrical activity at the measurement site) and high frequencies (Such as movements made by the subject among other things). When the topic of these frequencies was considered, the lower end cutoff and the upper end cutoff were determined to be 10Hz and 450Hz respectively. In order to achieve this we constructed a high pass and low-pass filter, this can be seen in the LTspice figure below.

![BandpassFilter](E:\Documents\School\Classes\BME Lab 1\Module 3\Bioinstrumentation Lab report 2\Procedure 3\BandpassFilter.PNG)

As can be seen from the graph above, the filter removes frequencies less than 10Hz and 450Hz as required. However, it is important to note that the minimum attenuation is -10dB. This is most likely as a result of the bandwidth being so narrow (440Hz). One possible solution to get rid of this is possibly to use an RL circuit or an active filter to make sure we are getting more precise filtration.



#### Secondary Amplification:

To ensure that the signals that are passed through our filter can accurately be processed by the ADC on the ATmega 328P microcontroller, we need to amplify this signal. Since the measurements seem to be around the order of 10s of millivolts, if we multiply by a $A \approx 100$, we should be able to get the voltage range we need. 

#### Final Circuit:

![Full EMG Circuit](E:\Documents\School\Classes\BME Lab 1\Module 3\Bioinstrumentation Lab report 2\Procedure 3\Full EMG Circuit.PNG)

