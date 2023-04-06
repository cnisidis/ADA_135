# ADA_135 C NISIDIS AVA.ADA 2207
Personal repository for course ADA_135 at Ionian University

## Preface
In recent years, I have been deeply concerned about the seismic activity in Greece. This concern led me to visualize data obtained from seismological recordings spanning several decades up to the present day.

As a next step, I began exploring the possibility of sonifying this data with the ultimate goal of presenting it in an interactive installation.

To accomplish this goal, I had to extensively study the phenomenon of earthquakes, the methods used to record them, and their geological significance.

Below I will list the steps and methods I followed, the tools I used or had to make for this purpose.


## Visualization and Sonification of Seismic Activity

This attempt consists of two distinct parts. In **Part I** (Lithosphere), we represent earthquakes as points in space, and in **Part II**, we discuss the possibility of directly sonifying data streams from seismographs.

***
### Part I: Lithosphere

<cite>Earthquakes as point-clouds</cite>

On the first part the goal is to visualize and represent seismic events (earthquakes) in the 3d or 2d space as a pointcloud.


|[![Lithosphere Video on youtube](lithosphere_00.png)](https://youtu.be/JKl6PjuqT88)|
|:--:|
|<small>*Youtube Video [Lithosphere](https://youtu.be/JKl6PjuqT88)*</small>|

##### Earthquake Event Keypoints:

- Location <small>(Longtitude & Lattitude)</small>
- Magnitude <small>(Richter)</small>
- Depth <small>(km)</small>
- DateTime <small>(UTC)</small>

In the current version of this project, each point inherits the properties of the corresponding earthquake event and also possesses the necessary properties and methods to visualize and sonify it.

#### Ways to Interpret and Correlate the data:

- Linearly with a fixed interval, each point is being added or lighten up after the other ignoring the time difference or the actual distance.
- Linearly with a fixed time interval, each point is added or illuminated after the other, ignoring the time difference or the actual distance.
  <small><i>Specifically, if we assume that event A occurs at t0 and event be at t1 then the interval is t0-t1. By this means it is expected to result in an output which has its own internal rhythm.</i></small>
- Non-linear using the centroids of the individual clusters. Events are separated into clusters using the Kmeans algorithm. By self-organizing the points based on their internal criteria it allows us to observe them in parallel threads, simultaneously.


##### Visualization Method

In the current application, the *saturation* of color represents the time of appearance, the hue of color represents the ratio of *magnitude* to *depth* (intensity), and the *magnitude* represents the size (radius) of each point, respectively.

##### Sonification Method

- Time distance is translated to the time interval for the MIDI triggers.
- Depth affects the tone (frequency)
- Magnitude affects the velocity
- and Location (physical distance among the events) is being transcribed to Reverb wetness.  


***
### Part II

<cite>Collect data in real-time (almost real time) from seismographs around the globe.</cite>

In this section, our goal is to sonify data by accessing a continuous stream derived from specific stations around the world. 

The inspiration for this idea comes from an audio experience where the listener stands in the center of a room surrounded by speakers. 

Each stream from the seismograph stations is audibly represented as a continuous sound signal emanating from the speakers placed around the room.


This part is segmented to distinct phases.

##### Phase A. Retrieve Data Sources

This section is generally more complex than Part II as the data is not always publicly available, making it difficult to obtain. However, the good news is that most of the data we need can be accessed through various networks depending on their location and the organization or institution that manages them.

By setting and pushing simple queries with this [builder](http://eida.gein.noa.gr/fdsnws/dataselect/1/builder) we are capable to retrieve useful data from the NOA (National Observatory of Athens) or directly acquiring data through the FDSN[^1] via IRIS[^20].

##### Phase B. Acquire Data

Initially, seismographs use the SEED format for exchanging seismic data (this is very depended on the sensors and the type of the data we request). 

Depending on the channels of the device we may have access to different streams of data. 

Each channel has an abbreviation code which signifies the type of the data and gives some hints regarding the capabilities of the sensor.

However, as we discussed previously, it is always possible to collect (request) data that concern the sensors and the instruments of each station.

The channels are mentioned in a MiniSEED record may follow the bellow abbreviations:
| Channel | Description |
|-:|:-|
|EHZ/EHN/EHE|Short Period 100 sps
|BHZ/BHN/BHE|Broad Band 20 sps
|LHZ/LHN/LHE|Long Period 1 sps
|VHZ/VHN/VHE|Very Long Period 0.1 sps
|BCI|Broad Band Calibration Signal
|ECI|Short Period Cal
|LOG|Console Log
|ACE|Administrative Clock Error
|LCQ|1hz Clock Quality
|OCF|Opaque Configuration File

In particular, there is a subversion of SEED[^10] called MiniSEED, which comes in different flavors, depending on the nature of the data and the capabilities of each Station[^5]

So far the most comon type of MiniSEED seems to be the STEIM2 compression. 

##### Phase C. Rectify and Resample Acquired Data

Upsampling from 100Hz to 48kHz, antialising order:15
no IFRs were applied

Listen to an audible test on [soundcloud](https://soundcloud.com/cnisidis/earthquake-stream)




##### Phase D. Render Data to Audio

There are several methods to convert a barely audible signal to an audio signal. One approach is upsampling, which can increase the range resolution by spreading out the data across the entire spectrum. Another option is to use pitch shifting with algorithms such as PSOLA or other OLA algorithms. However, I have decided to opt for a simpler, less precise method that is more artistically interesting - using the signal envelope.

Process of creating the envelopes:
- Obtain the FFT of the signal.
- Map and clamp the FFT bins or bands.
- Send the mapped and clamped bins as envelopes through MIDI.
- Use these envelopes as input in VCVRack.

//TODO waveform example

//TODO FFT Envelopes Example

Even though the signal's sampling rate is 100Hz/sec in our example, we are still able to read it and obtain its FFT. This allows us to generate new envelopes that can be used to drive oscillators.

For the time being, we will create our oscillators in VCVRack, although any other digital audio workstation (DAW) or programming language such as PD, MAX/MSP, or SoundCollider can also be used.

| ![VCVRack Oscillators](VCVRack_Oscillators.png) |
|:--:|
| <small>*In the example above, I am using 12 FFT bands to generate envelopes, which will drive 12 different oscillators. Each oscillator is tuned in such a way that it maintains the average range of each band.*</small> |


//TODO Multichannel Audio 



##### Phase E. Represent Data graphically (optional)
//TODO








[^1]: [International Federation of Digital Seismograph Networks](http://www.fdsn.org/)
[^5]: Station: Every potential seismographic device which respects the FDSN convention. A Station has some standard characteristics such as a code, a channel, a network and a location.
[^10]: [miniSEED](https://ds.iris.edu/ds/nodes/dmc/data/formats/miniseed/)
[^20]: [IRIS](http://service.iris.edu/)
