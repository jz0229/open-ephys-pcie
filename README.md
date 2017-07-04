## Open Ephys PCIe 

Hardware, firmware, and host APIs for serialized, very-high channel count
electrophysiology (recording and stimulation).

__Maintainer__: jonnew

__Note__ This work is a second-order fork. It is based on the following open
source designs:

1. [Intan's headstages](http://intantech.com/index.html): no license provided.
2. [Open Ephys headstages](https://github.com/open-ephys/headstage): Open
   Hardware license. Based upon (1).

### Description
In traditional neural recording systems, singals are amplified by a headstage
which serve as a buffer between high-impedance electrodes recording signals
from the brain and low-impedance wires that carry signals to a rack-mounted
filter bank and amplifier array. There the signals are digitized and sent to a
computer for processing and storage.  In this "one physical connection per
signal" scenario, auxiliary sensors (e.g. inertial measurement units,
indication LEDs, etc) and brain stimulator devices (e.g. electrical
microstimulation or optical inputs) each mandate an additional electrical or
optical connection to the animal subject, increasing tether weight and tendency
to failure.

The goal of this project is to take advantage of commodity integrated circuits
and FPGAs in order to reduce physical connectorization to a single, miniature
coaxial cable while maintaining high channel counts (up to 1000).  Further, we
sought to include sufficient onboard computation power in orchestrate data
acquisition and generate stimulation patterns. To this end, we present a set of
free and open-source circuit, firmware, and API modules designs allowing very
high channel count electrophysiology, microstimulation, optogenetics,
behavioral tracking, and auxiliary sensor acquisition in freely moving animals.
A major common feature of the designs in this project is that communication and
power between the animal and host computer is facilitated by a single miniature
coaxial cable. Further, we provide host-side hardware and firmware required to
seamlessly integrate our system with several widely used free and open-source
data equations platforms (The Open Ephys GUI and Bonsai). Unlike previous
designs, our device makes use of the PCIe bus to transfer data and therefore
achieves submillisecond round-trip latencies to host-PC main memory.

### Major Features

- Integrated electrophysiology (up to 1000 channels), optogenetic or electrical
  micro stimulation (i.e. no fiber optic tether), 6 DOF pose measurement, etc.
- Submillisecond round-trip communication with host PC's main memory
- Data, user control, and power transmitted over a single coaxial cable
- Modular design allows custom integration of individual project components.
- Integrated electrode plating and impedance testing
- Quality documentation and easy routes to purchased assembled devices.
- Low profile, circular form factor headstage design which minimizes torque.

The designs in this project enable acquisition from up to 256 recording
electrodes, 12 auxiliary inputs (which can be user specified or dedicated to
onboard 9-axis pose sensing system), integrated LED driver for optogenetic
stimulation and/or constant current electrical stimulation (no fiber optic tether required).

## System Components
The modules of this repository, each corresponding to a top-level directory of the same name, is described below.

### Hardware Components

#### EIB-128
128 Channel electrode interface board. Designed for rat tetrode electrophysiology.

#### EIB-256
256 Channel electrode interface board. Designed for rat tetrode electrophysiology.

#### Spacer
Spacer to raise the stack height between board layers.

#### Headstage-128
A low profile 128-channel digital headstage module for amplifying, filtering, and digitizing
microelectrode voltage data from a rat microdrive implant. Up to 256 wires (64 tetrodes)
can be acquired by stacking two modules.

#### Test Board
Test board for headstage modules. Allows injecting simulated biopotentials into
headstage modules via a selectable passive attenuator.

#### SERDES Interface Board
Data serailization board for headstage-128. Designed for rat tetrode electrophysiology.

#### PCIe Host
Base board for facilitating PCIe communication, via KC705 or similar, with host
computer. This board fits into an empty PCIe slot and communicates with KC705
via an FMC ribbon cable.

#### Drive CAD
CAD designs for microdrive arrays that work with this system.

#### LED Board
Simple board for making pig-tailed, drivable stimulation LEDs for optogenetic
manipulation.

### Software/Firmware Components

#### oepcie
Open Ephys host library for creating applications that acquire data from
hardware in this project.

## Bill of materials
The bill of materials for all components can be found on [this google
doc](https://docs.google.com/spreadsheets/d/1F-KWcdvH_63iXjZf0cgCfDiFX6XXW3qw6rlR8DZrFpQ/edit?usp=sharing).

## Application example
The designs in this repository for them the basis for microdrive rat recordings
in the [Wilson Lab at MIT](http://web.mit.edu/wilsonlab/).

## Hardware Licensing
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img
alt="Creative Commons License" style="border-width:0"
src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br /><span
xmlns:dct="http://purl.org/dc/terms/" property="dct:title">Rat digital headstage</span> by <a xmlns:cc="http://creativecommons.org/ns#"
href="https://github.com/jonnew/cyclops" property="cc:attributionName"
rel="cc:attributionURL">Jonathan P. Newman</a> is licensed under a <a
rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative
Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.<br
/>Based on a work at <a xmlns:dct="http://purl.org/dc/terms/"
href="https://github.com/jonnew/headstage"
rel="dct:source">https://github.com/jonnew/headstage</a>.

## TODO (not up to date...)
- [ ] Each component needs a README with a more detailed explanation of what it
  is and how it is used.
- [ ] I need to make sure that he DF40 headers are not going to short onto the
  vias underneath the connectors on the EIB. This should be done empirically.
  - EDIT: they do not short, but there is a different problem: one of the gold
    pins on TT23 of the EIB hits the outer edge of the DF40 receptical above
    it. This pin needs to be moved. More generally, I need to add vertical
    keepouts to the DF40 parts to prevent this in the future.
- [ ] It is very hard to get the fab to make the right via size on the 256
  channel EIB. These need to be precisely as specified or the gold pins will
  not fit or fall through.
