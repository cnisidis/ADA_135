# ADA_135 C NISIDIS AVA.ADA 2207
Personal repository for course ADA_135 at Ionian University

## Visualization and Sonification of Seismic Activity

The present attempt is being consisted of two deistinct parts.
In the <b>Part I</b> (Lithosphere) we are representing earthquakes as points in space and in the <b>Part II</b> we discuss the possibility of sonifying data stream directly from Seismographs.

***
### Part I: Lithosphere

<cite>Earthquakes as point-clouds</cite>

On the first part the goal is to visualize and represent seismic events (earthquakes) in the 3d or 2d space as a pointcloud.


[![Lithosphere Video on youtube](lithosphere_00.png)](https://youtu.be/JKl6PjuqT88)
<small>Youtube Video [Lithosphere](https://youtu.be/JKl6PjuqT88)</small>

##### Earthquake Event Keypoints:

- Location <small>(Longtitude & Lattitude)</small>
- Magnitude <small>(Richter)</small>
- Depth <small>(km)</small>
- DateTime <small>(UTC)</small>

In the current state of this version of the project, each Point inherits the properties of its respective event (Earthquke) plus the properties and the methods it needs to be visualized and sonified.

#### Ways to Interpret and Correlate the data:

- Linearly with a fixed interval, each point is being added or lighten up after the other ignoring the time difference or the actual distance.
- Linearly with a variant interval, each point is being added or lighten up after the other respecting their time difference and their physical distance.
  <small><i>In particular, assuming that event A happens in t0 and event be in t1 then the interval is t0-t1. By this mean it is expected to come up with an output which has its own internal rythm.</i></small>
- Non-Linearly by using the centroids of individual clusters. The events are being separated to clusters with Kmeans algorithm. By self-organising the points with their internal criteria it gives us the opportunity to observe them in parallel threads, simultaneously.


##### Visualization Method

In the current implementation, color's saturation depicts the time of the occurence, the color's hue the ratio of the magnitude over the depth (intensity), and the magnitude the size(radius) of each point respectively.

##### Sonification Method

- Time distance is translated to the interval for the MIDI triggers.
- Depth affects the tone (frequency)
- Magnitude affects the velocity
- and Location (physical distance among the events) is being transcribed to Reverb wetness.  


***
### Part II

<cite>Collect data in real-time (almost real time) from seismographs around the globe.</cite>

On this part, our goal is, basically, to sonify data by accessing a continuous stream deriving from specific stations around the world.

The idea draws its inspiration by an audio experience where the auditor stands in the center of a room which is surrounded by speakers.

Each stream from each station (seismograph) is represented audibly as continuous sound signal coming through the spreaded speakers in the room.


This part is segmented to distinct phases.

##### Phase A. Retrieve Data Sources

This attempt in general turns to be more complicated than Part II, since the data are not always publically available and it is quite harsh to attain them.
On the other hand, the good part, is that most of the data we are looking for are accessible via different Networks, depending their localization and the foundation or affiliation handles them.

By setting and pushing simple queries with this [builder](http://eida.gein.noa.gr/fdsnws/dataselect/1/builder) we are capable to retrieve useful data from the NOA (National Observatory of Athens) or directly acquiring data through the FDSN[^1]

##### Phase B. Acquire Data

Initially most of the devices out there are using SEED format for data exchanging. 

In particular, there is a subversion of SEED[^10] called MiniSEED which comes to different flavors, depending on the nature of the data and the capabilities of each Station[^5]

So far the most comon type of MiniSEED seems to be the STEIM2 compression. 

##### Phase C. Rectify and Resample Acquired Data

Upsampling from 100Hz to 48kHz, antialising order:15
no IFRs were applied

Listen to an audible test on [soundcloud](https://soundcloud.com/cnisidis/earthquake-stream)




##### Phase D. Render Data to Multichannel Audio
//TODO

##### Phase E. Represent Data graphically (optional)
//TODO








[^1]: [International Federation of Digital Seismograph Networks](http://www.fdsn.org/)
[^5]: Station: Every potential seismographic device which respects the FDSN convention. A Station has some standard characteristics such as a code, a channel, a network and a location.
[^10]: [miniSEED](https://ds.iris.edu/ds/nodes/dmc/data/formats/miniseed/)
