# AM-Reciever-Module-with-LTspice-Simulation-for-a-Human-Voice-
The aim of this Project is building an AM Reciever Circuit and Simulate the Carrier Wave for a Human Voice Modulated of 1.5 Mhz in LTspice as a Carrier Wave and extract the real Voice into a .Wav File to Simulate Input/Output of a Radio

# Input Data
an Input Sound of Human Voice is used that has a duration of 8 seconds 

[mip.wav](https://github.com/user-attachments/files/22076028/mip.wav)

# PWL Conversion

The Sound is Converted to a PULSE WISE LINEAR File which is a Combination of a value in term of Time. The Values are Normalized to be in the [-1 , 1] Range, a Stereo Voice is changed to a Mono to be trated later by LTSpice as Shown in the Code
[zfe.txt](https://github.com/user-attachments/files/22076099/zfe.txt)

import numpy as np
from scipy.io import wavfile

rate, data = wavfile.read("mip.wav")

# IF STEREO WE MAKE IT MONO

if len(data.shape) > 1:
    data = data.mean(axis=1)

# TO NORMALIZE THE VALUES [ -1, 1]

with open("output.pwl", "w") as f:
    for i, sample in enumerate(data):
        time = i / rate
        voltage = sample / np.max(np.abs(data))  
        f.write(f"{time:.6f} {voltage:.6f}\n")

# Class A Amplifier 

by fixing the Biasing to obtain VCE = VCC/2 so to maximise the Swing in the Quiscent Point and prevent the Oscillation from the Distorion in term of Cut Off and Saturation. We obtained an Amplified Voice. it's Better to Use a Cascode Transistors in Term of Building the Physical Circuit to obtain better stabilisation when isolationg the Input from the Output, because Class A amplifier working for 360 degree and permit Heating 
The Figure shown the Blue wave as the Input Voice and the Green is the Amplified Voice:
<img width="1366" height="695" alt="regegerge" src="https://github.com/user-attachments/assets/36a701bc-aba7-4da9-b1bd-bbdc6ceeeb41" />

[bmw.wav](https://github.com/user-attachments/files/22076243/bmw.wav)

# Modulation 
Modulate the Voice using the Famous Equation V(Modulation) = V(carrier) * ( 1 + m * V(message)) which m is the Modulation Factor m = V(message_max)/ V(carrier_max) , when m>1 it's an Overmodulation but when m<0 it's an UnderModulation. m=0.5 in this Project than it's multiplied by 10 because of The PWL Conversion hat reduced the Values of the Sound to very Smaller Values devided by 10.

# Circuit Stages 

The Circuit Compromise an LC Tank in resonane to choose the wanted frequency than a rectifier with a Bleader Resistor to permit the Discharge of the Capacitor it's Crucial to choose the (1/frequency of the Carrier) <RC < (1/frequeny of the Message) , so the Smoothing Capacitor can attend the Enveloppe Variation when the Charging and Descharging Process, also a bigger RC in That Range can Perform better to reduce the Distortions for better Stability. So we can Play around thoses Factors to Ameliorate the Wave , the Class A amplifier is used in Case of Improving the Current and Isolate Stages but we could use a Cascode Transistors for better Stability and Amelioration of Voice Quality.
RC(Doucpling/Filtering) to eliminate AC Waves in VCC Line.
Class AB Amplifier : The two Diodes permit Biasing of the Push Pull Transistors.
An Ideal Voltage Dependent Source with a Series Resistance to imitate the Impedance of real Circuit, The IVDS hat a very Huge Input Impedance and a Very Low Output Impedance which is adequate to Visualise the Plot Wave , in Our Case the Tracing of the Wave took 2 Hours to take a Part of the Voice because of the 44.k numbers of Samples which is Huge for LTSpice Simulation

The Red is the Carrier Wave and the Green the Output Voice

<img width="1366" height="699" alt="zdsfsd" src="https://github.com/user-attachments/assets/a898c927-3927-4eed-93fa-f92c3bc45a01" />

Then Taking a closer look to the Wave 
The Modulation and extraction of the Enveloppeis mehr Clear
<img width="1365" height="697" alt="fdgdfgd" src="https://github.com/user-attachments/assets/bd3c723e-0a7d-4553-a18e-d85c4057acfb" />

The Taking a mehr Closer Look 
Hier is mehr Clear and showing nicely the Modulation, The Blue is the Wave directly after the Rectifier.
<img width="1366" height="694" alt="gfnfnfdn" src="https://github.com/user-attachments/assets/0da35866-3d0f-4b5b-b689-5058a566df0a" />

The Output Voice  :

[eee.wav](https://github.com/user-attachments/files/22076697/eee.wav)


