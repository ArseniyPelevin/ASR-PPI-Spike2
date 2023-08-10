# Spike2 Script for Acoustic Startle Response (ASR) and Prepulse Inhibition (PPI) Protocols

## Discription

[Prepulse inhibition](https://en.wikipedia.org/wiki/Prepulse_inhibition) (PPI) of acoustic startle response (ASR) is 
a neurological phenomenon in which a weaker audial prestimulus inhibits startle reaction of an organism 
to a subsequent strong stimulus.

This Spike2 script enables acoustic PPI animal tests with data acquisition 
and control interface [CED Power1401](https://ced.co.uk/products/pow3in) (Cambridge Electronic Design, Cambridge, UK)
or a similar Spike2-based equipment.

Procedure is a series of acoustic white noise pulses of different amplitudes:
- *prepulses* - quieter prestimuli 
- *pulses* - louder stimuli evoking startle reflex
- *paired pulses* - pairs of prepulse+pulse to measure prepulse inhibition
Pulses are spaced with background white noise.

The script allows two modes of stimuli presentation: automatic and manual.  
**Automatic mode** - experimenter can set time interval between pulses (intertrial interval - ITI), number of stimuli
of each type, sequence of blocks of same-type stimuli and time of habituational background white noise
before the start of experiment.  
**Manual mode** - experimenter triggers stimuli themself with keyboard:  
**r** - prepulse, **p** - pulse, **d** - double

It is possible to save current settings and load them again in future experiments.

The script was validated and published in a peer-reviewed article. The article contains additional descriptions of its functioning:  
Pelevin, A., Kurzina, N., Zavialov, V., & Volnova, A. (2023). A Custom Solution for Acoustic Startle Response Setup with Spike2-Based Data Acquisition Interface. Methods and Protocols, 6(3), 57. https://doi.org/10.3390/mps6030057

Other neurophysiological articles using this script:  
https://doi.org/10.3390/biomedicines11010222  
https://doi.org/10.3390/biom12101484

## Interface and parameters

![Interface of the Settings](https://www.mdpi.com/mps/mps-06-00057/article_deploy/html/images/mps-06-00057-g003.png)

## Hardware

Components of the setup equipment necessary for the procedure:  
- **CED Power1401** data acquisition and control interface controlled with **Spike2** software
- Sound amplifier
- Loudspeakers installed in the walls of the cage with animal
- Accelerometers mounted in the platform beneath the cage
  
![Block diagram of the ASR system components](https://www.mdpi.com/mps/mps-06-00057/article_deploy/html/images/mps-06-00057-g001.png)

## Volume calibration

Data acquisition and control interface's output is electrical current of a range of voltage (usually ±5V). 
However, real sound level depends on specific hardware, including sound amplifier settings, loudspeakers, and cage acoustics. 
The `Db2Vol` function converts custom sound volume settings in decibels to the linear range of 1–100. 
The function will further convert this value into the amplitude of the output voltage on the digital–analog channel.

`db` - sound level in decibels set by user  
`volume` - percent of the maximal output voltage amplitude on the given apparatus  
`amplitude` - output voltage amplitude

Calibration:
1.  Assign  
    `var volume := db;`
2.  Set **Background volume** in the *Settings* dialogue box in the range 1-100 and measure the actual sound volume in the apparatus' chamber with a sound level meter  
    ! It is important to measure the sound level on a steady noise rather than short pulses, as the time of the pulse might not be enough for a sound level meter to measure it correctly
    1.  Set **Background volume** as 100.
        If you use a sound amplifier, set its own volume to the lowest level
        that yields the maximal sound volume necessary in the experiment (e.g. 120 db)  
    2.  Set **Background volume** in the range 1-100 with a small step.
        For each value measure the corresponding sound level  
        ! It may be necessary to adjust the sound level meter's settings 
        to different sound volume ranges
4.  Plot sound level as a function of the **Background volume** in the range 1-100.
    Make a logarithmic approximation (e.g. in the MS Excel)
5.  Take the values of `a` and `b` from the resulting equation in the format   
    $y=a*ln(x)+b$, where $y=db$, and $x=volume$.
6.  Assign the values to the variables `a` and `b` below, 
    reassign `volume` to the formula

```vb
func Db2Amp(db)

var a := 9.7861;
var b := 72.061;

'Comment for calibration:
var volume := Pow(_e,(db-b)/a);
'Uncomment for calibration:
'var volume := db;

var maxVoltage := 2;    'Maximal output voltage on the given apparatus
var amplitude := maxVoltage * volume / 100; 

return amplitude;
end;
```

## Future development

Features that could be added to this script:
- Pure tone stimuli of customizable frequency
- Different number of signals in each block of stimuli

## Credits

This software was developed as part of my research work in the Laboratory of Neurobiology and Molecular Pharmacology of the Institute of Translational Biomedicine of St Petersburg University under the scientific supervision of Dr. Anna B. Volnova <a href="https://orcid.org/0000-0003-0724-887X"><img alt="ORCID logo" src="https://info.orcid.org/wp-content/uploads/2019/11/orcid_16x16.png" width="16" height="16" /></a>

This work was supported by the Russian Science Foundation grant number 21-75-20069.

