#Prepulse inhibition procedure

#Discription

[Prepulse inhibition](https://en.wikipedia.org/wiki/Prepulse_inhibition/) (PPI) - 
neurological phenomenon in which a weaker prestimulus inhibits the reaction of an organism 
to a subsequent strong reflex-eliciting stimulus, often using the startle reflex.

This Spike2 script enables acoustic PPI animal tests through data acquisition 
and control interface [CED Power1401](https://ced.co.uk/products/pow3in).\

Procedure is a series of acoustic white noise pulses of 50-115 dB:
- _prepulses_ - quieter prestimuli 
- _pulses_ - louder stimuli evoking startle reflex
- _paired pulses_ - pairs of prepuls-puls to observe prepulse inhibition
Pulses are spaced with background white noise of 50 dB.

Script allows two modes: automatic and manual. 
__Automatic mode__: experimenter can set time interval between pulses (intertrial interval - ITI), number of stimuli
of each type, sequence of blocks of same-type stimuli and time of habituational background white noise
before the start of experiment.
__Manual mode__: experimenter triggers stimuli themself with keyboard:\
__r__ - prepulse, __p__ - pulse, __d__ - double

#Interface and parameters

_italic_ - for automatic mode only
[] Enable automatic mode
- __Background volume (50-115 dB)__ - volume of background white noise
- _Noise duration (min)_ - time of habituational white noise before experiment

- __Prepulse volume (50-115 dB)__
- __Prepulse duration (0-100 ms)__
- _Number of prepulses_ - (in one block)

- __Pulse volume (50-115 dB)__
- __Pulse duration (0-100 ms)__
- _Number of pulses_ - (in one block)

- __Prepulse-pulse interval (ms)__
- _Number of prepulse-pulse pairs_ - (in one block)

- _Inter-trial interval (1-30 s)_ - time between signals of any type
- _ITI variation (0-100%)_ - time between signals should vary to stay unexpected

- _Sequence of blocks_ - Experimenter can enter any number of blocks in any order
__R - pRepulse, P - Pulse, D - Double__

# Volume calibration

Data acquisition and control interface's output is electrical current of a range of
voltage (usually ±5V). However, real sound depends on speaker, amplifier and setting configuration. 
Volume (0-100) to dB conversion function should be calibrated with sound level beter on place.

```
func Db2Vol(db)     
var vol := Pow(_e,(db-72.061)/9.7861);	' Comment for calibration
'var vol := 0;					' Uncomment for calibration
return vol;
end;
```
Calibration:
1. Change commented line as hinted in the script above
2. Run script with sound levels in range 0-100 (0 will be background noise in your lab)
3. Measure real sound levels in dB with sound level meter
4. Build calibration curve in MS Excel
5. Build logarithmic approximation, obtain the equasion (you should get smth like __dB = 9,7861ln(vol) + 72,061__)
6. Express __vol__ in terms of __dB__ and update furmula in the script

# Future development

Features that can be added to this script:
- Single tone stimuli of customized frequency
- Different number of signals in each block of stimuli