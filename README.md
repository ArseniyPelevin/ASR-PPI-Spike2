# Prepulse inhibition procedure

# Discription

[Prepulse inhibition](https://en.wikipedia.org/wiki/Prepulse_inhibition/) (PPI) - 
neurological phenomenon in which a weaker prestimulus inhibits the reaction of an organism 
to a subsequent strong reflex-eliciting stimulus, often using the startle reflex.

This Spike2 script enables acoustic PPI animal tests through data acquisition 
and control interface [CED Power1401](https://ced.co.uk/products/pow3in).

Procedure is a series of acoustic white noise pulses of 50-115 dB:
- *prepulses* - quieter prestimuli 
- *pulses* - louder stimuli evoking startle reflex
- *paired pulses* - pairs of prepuls-puls to observe prepulse inhibition
Pulses are spaced with background white noise of 50 dB.

Script allows two modes: automatic and manual. 
**Automatic mode** - experimenter can set time interval between pulses (intertrial interval - ITI), number of stimuli
of each type, sequence of blocks of same-type stimuli and time of habituational background white noise
before the start of experiment.
**Manual mode** - experimenter triggers stimuli themself with keyboard:  
**r** - prepulse, **p** - pulse, **d** - double

# Interface and parameters

*italic* - automatic mode only  
- [ ] Enable automatic mode
- **Background volume (50-115 dB)** - volume of background white noise
- *Noise duration (min)* - time of habituational white noise before experiment <br/><br/>

- **Prepulse volume (50-115 dB)**
- **Prepulse duration (0-100 ms)**
- *Number of prepulses* - (in one block) <br/><br/>

- **Pulse volume (50-115 dB)**
- **Pulse duration (0-100 ms)**
- *Number of pulses* - (in one block) <br/><br/>
  
- **Prepulse-pulse interval (ms)**
- *Number of prepulse-pulse pairs* - (in one block) <br/><br/>
  
- *Inter-trial interval (1-30 s)* - time between signals of any type
- *ITI variation (0-100%)* - time between signals should vary to stay unexpected <br/><br/>  
  
- *Sequence of blocks* - Experimenter can enter any number of blocks in any order  
*R - pRepulse, P - Pulse, D - Double*

# Volume calibration

Data acquisition and control interface's output is electrical current of a range of
voltage (usually ±5V). However, real sound depends on speaker, amplifier and setting configuration. 
Volume (0-100) to dB conversion function should be calibrated with sound level beter on place.

```
func Db2Vol(db)     
var vol := Pow(_e,(db-72.061)/9.7861);    ' Comment for calibration
'var vol := 0;                            ' Uncomment for calibration
return vol;
end;
```

Calibration:
1. Change commented line as hinted in the script above
2. Run script with sound levels in range 0-100 (0 will be background noise in your lab)
3. Measure real sound levels in dB with sound level meter
4. Build calibration curve in MS Excel
5. Build logarithmic approximation, obtain the equasion (you should get smth like **dB = 9,7861ln(vol) + 72,061**)
6. Express **vol** in terms of **dB** and update furmula in the script

# Future development

Features that can be added to this script:
- Single tone stimuli of customized frequency
- Different number of signals in each block of stimuli
