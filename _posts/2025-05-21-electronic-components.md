---
layout: post
title: "Electronic Components"
date: 2025-05-21
tags: ["Networking"]
---

Computer networking is about connecting two or more computers together

---
### Introduction


---
### Components

**Resistor**

**Capacitor**

**Inductor**

**Transistor**

**BJT**

**Diode**


---

### Some Important Concepts

**Modulation and Demodulation**

- Modulation is the process of encoding information into a transmitted signal
    - converting digital signal into analog signal is called Modulation
    - Amplitude Modulation (AM), Frequency Modulation (FM)
    - e.g. In radio braodcasting, audio signals are modulated onto a radio frequency carrier wave
- Demodulation is the process of extracting information from the transmitted signal
    - converting analog signal into digital signal is called Demodulation
    - e.g. In radio reception, a receiver demodulates the radio frequency carrier wave to extract the original audio signal

**Modulation Techniques**

_DSSS_

- Direct Sequence Spread Spectrum
- e.g. 802.11b

_OFDM_

- Orthogonal Frequency Division Multiplexing
- e.g., 802.11a/g/n/ac/ax

_QAM_

- Quadrature Amplitude Modulation
- used in OFDM subcarriers

**Line Coding Schemes**

- e.g. Manchester Encoding, 8B/10B encoding, 128B/130B encoding

**Hardware Multiplexing**

- It takes several input signals and combines them into a single output signal for transmission over a shared channel
- (Analogy) multiple phone calls combined to transmit over one single wire

<img src="{{site.url}}/images/networking/mux.png">

_Working Principle - 2-to-1 Multiplexer_
- The multiplexer works like a multiple-input and single-output switch. 
- The output gets connected to only one of the n data inputs at a given instant of time. 
- Uses Time-Division Multiplexing (TDM) to divide time into tiny slots and the MUX selects one input line at each time slice
- However it looks like multiple streams are flowing simultaneously because the switching happens very fast at GHz or MHz scale
- Therefore, the multiplexer is ‘many into one’ and it works as the digital equivalent of an analog selector switch

<img src="{{site.url}}/images/networking/mux_21.png">

_Implementation_
- Multiplexer is a digital electronic circuit which is implemented in IC by logic gate 
- When S=0,
    - first AND Gate gets D0 and S=1 which outputs D0
    - second AND Gate gets D1 and S=0 which outputs 0
    - D0 OR 0 => D0
- When S=1,
    - first AND Gate gets D0 and S=0 which outputs 0
    - second AND Gate gets D1 and S=1 which outputs D1
    - 0 OR D1 => D1

| S | Y |
| 0 | D0 |
| 1 | D1 |

<img src="{{site.url}}/images/networking/mux_21_logic_gate.png">

**Software Multiplexing**

- It allows multiple applications on a host to simultaneously send and receive data by assigning each connection a unique source and destination port
- It is done at the Transport Layer

**RF Front-end**

- the part of a wireless NIC (like in Wi-Fi cards) that handles the transmission and reception of radio signals via the antenna
- Components
    - Antenna - Radiates and receives electromagnetic (RF) waves
    - Low Noise Amplifier (LNA) - Boosts weak incoming signals without adding much noise
    - Power Amplifier (PA) - Strengthens outgoing signals before transmission
    - Filters - Remove unwanted frequencies and interference (bandpass, low-pass, etc.)
    - Mixers - Convert RF signals to lower-frequency intermediate frequencies (IF) or baseband for digital processing
    - Switches/Duplexers - Allow sharing one antenna for both transmit and receive paths
