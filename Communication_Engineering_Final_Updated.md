# Communication Engineering — Complete Simplified Study Notes
*Condensed from Lectures 01–05. Everything you need is in this file — formulas, definitions, examples, and worked problems.*
**⭐ = appears in the 2023 and/or 2024 previous-year question papers**

---

# UNIT 1 — Introduction to Communication Systems

## What is Communication?
Communication is the process of establishing a connection between two points to exchange information (voice, text, images, data, etc.).

## ⭐ Block Diagram of a Communication System
```
Information source → Input transducer → Transmitter (Modulator) → CHANNEL (+Noise) → Receiver (Demodulator) → Output transducer → Destination
```
| Block | Function |
|---|---|
| Information source | Produces the message: voice, music, pictures, text, data, etc. |
| Input transducer | Converts the physical message into a time-varying **electrical** signal (e.g. microphone, camera, scanner) |
| Transmitter | Performs frequency-range restriction, amplification and **modulation**. |
| Channel | The path through which the signal travels from transmitter to receiver. |
| Noise | Unwanted random signal added to the desired signal. |
| Receiver | Extracts the original message from the degraded received signal. |
| Output transducer | Converts the electrical message back to the original physical form (e.g. loudspeaker). |
| Destination | Final point where the information is obtained. |

## Basic Modulator Block Diagram

A simple modulator arrangement shown in the class note is:

```text
Baseband / Message Signal → Modulator → RF Power Amplifier → Antenna
                              ↑
                         Carrier / RF Oscillator
```

The modulator combines the message signal with the high-frequency carrier. The RF power amplifier raises the transmitted RF power.

## Bandwidth
**Bandwidth = the range of frequencies a signal occupies = f₂ − f₁** (upper − lower frequency).
- Example: music signal spans 20 Hz–15 kHz → BW = 15000 − 20 = **14,980 Hz**
- Typical signal bandwidths: Speech 300 Hz–3100 Hz | Music ~15 kHz | Video 0–5 MHz

## ⭐ Communication Channels — Properties & Comparison
## Channel Selection — Practical Considerations

The class note lists these points when selecting a communication channel:

- Bandwidth
- Data rate
- Noise immunity
- Cost
- Reliability
- Security
- Scalability

### Three Common Guided Media

| Feature | Twisted pair | Coaxial cable | Optical fiber |
|---|---|---|---|
| Transmission medium | Copper wires twisted together | Copper conductor with shielding | Glass/plastic fiber carrying light |
| Signal type | Electrical | Electrical | Light |
| Bandwidth | Low to medium | Medium to high | Very high |
| Data speed | Low to medium | High | Very high |
| Distance | Short | Medium | Long |
| Cost | Low | Medium | High |

The class note identifies twisted-pair, coaxial and optical-fiber cables as three major guided communication channels.

### Why Twisted-Pair Conductors Are Twisted

The class note explains that twisting reduces electromagnetic interference and crosstalk between adjacent wires, minimizes interference from nearby electromagnetic fields, and improves signal quality.

A channel is judged on: power needed for desired S/N, bandwidth, amplitude/phase response, linear or non-linear, susceptibility to interference.

| Channel | Key facts | Advantages | Disadvantages |
|---|---|---|---|
| **Telephone channel (twisted pair)** | Bandpass 300–3400 Hz, SNR ≈ 30 dB, nearly linear | Cheap | Limited bandwidth |
| **Coaxial cable** | 75Ω/50Ω types; carries 10,000+ voice channels; repeaters every ~1 km; 8.5–274 Mb/s | Noise-immune (shielded), large BW, low loss | More expensive than twisted pair (cheaper than fiber) |
| **Optical fiber** | Thin glass/plastic core, carries light | Small & light, cheap to run, **no EM interference**, very large BW, long distance | High initial/maintenance cost, fiber losses, jointing is tricky |
| **Terrestrial microwave** | Needs line-of-sight; unidirectional | No cables needed, wide BW, multiple channels | Signal weakens from multipath; can't penetrate walls |
| **Satellite** | Relay station in space; uplink/downlink frequencies (e.g. 6 GHz up / 4 GHz down); geostationary height ≈ 35,683 km | Covers huge areas | Very high build/launch cost |

**→ Optical fiber is usually the "most advantageous" channel** (huge bandwidth + immune to interference), despite the higher cost.

## ⭐ Classification of Communication Systems
**1) By direction:**
| Type | Meaning | Example |
|---|---|---|
| Simplex | One direction only | Radio/TV broadcast |
| Half-duplex | Both directions, but not at the same time | Walkie-talkie |
| Full-duplex | Both directions, simultaneously | Telephone |

**2) By signal type — Analog vs Digital**
| | Analog | Digital |
|---|---|---|
| Signal | Carrier's amplitude/phase/frequency varies continuously with message | Transmitted as pulses of fixed amplitude/frequency/phase |
| Examples | AM, FM, PM, PAM, PWM, PPM | PCM, DM |
| Advantages | Simple Tx/Rx, low bandwidth, FDM possible | Noise-resistant, easy multiplexing/encryption, reliable, voice+data integration |
| Drawbacks | Noise degrades signal and can't be separated out; no repeaters between Tx/Rx; not secure | Needs more bandwidth; needs synchronization |

**3) By transmission technique:** Baseband (signal sent as-is — e.g. telephone network, but can't travel far/through free space) vs **Modulated transmission** (message signal is carried on a high-frequency carrier).

### Multiplexing and Digital Modulation Names from the Class Note

The class note lists **FDM** with analog communication and **FDM, TDM, and CDM** with digital communication. It also gives **ASK, FSK, and PSK** as examples of digital modulation. These names are included for recognition only; the uploaded five lecture decks do not develop their detailed theory.

## ⭐ What is Modulation & Why Do We Need It?
**Modulation** = varying a property (amplitude, frequency, or phase) of a high-frequency **carrier** signal according to the **modulating (message) signal**.

**Why we need it (5 reasons):**
1. **Reduces antenna height** (see worked example below)
2. Avoids mixing/interference of signals from different sources
3. Increases the range of communication
4. Enables multiplexing (many signals sharing one channel)
5. Improves quality of reception

### ⭐ Worked Example — Why modulation reduces antenna height
Minimum practical antenna height ≈ **λ/4 = c/(4f)**
- Baseband signal f = 10 kHz → height = (3×10⁸)/(4×10×10³) = **7,500 m (7.5 km)** → impractical
- Modulated signal f = 1 MHz → height = (3×10⁸)/(4×10×10⁶) = **75 m** → practical ✅

## Modulating Signal and Carrier Signal

- **Modulating/message signal:** contains the original information to be transmitted; its frequency is normally much lower than the carrier frequency.
- **Carrier signal:** a high-frequency sinusoidal signal that carries the information to the receiver after modulation.

The carrier frequency is chosen much higher than the modulating-signal frequency so practical transmission becomes possible.

## Types of Modulation (quick overview)
| Type | What varies | Notes |
|---|---|---|
| AM | Carrier amplitude | Frequency constant |
| FM | Carrier frequency | Amplitude constant |
| PM | Carrier phase | Amplitude constant; used to generate stable FM |
| PAM | Pulse amplitude | Pulse-based |
| PWM | Pulse width | Pulse-based |
| PPM | Pulse position | Pulse-based |

## Additional Digital Communication Points from the Class Note

The class note also emphasizes these practical advantages of digital communication:

- High noise immunity
- Easy error detection and correction
- High security
- Easy storage and processing
- Multimedia support

The class note also lists these disadvantages:

- Higher bandwidth requirement
- More complex equipment
- Higher initial cost
- Synchronization required
- Power consumption

The lecture itself also identifies sampling error, synchronization, and higher bandwidth as disadvantages.

## Limitations of Communication Systems
- **Bandwidth limitation** — available bandwidth caps maximum signal speed.
- **Noise and interference** — degrade the desired signal.
- **Limited bandwidth** — limits transmitted information.
- **Transmission delay** can occur.
- **Security risks** may exist.
- **Equipment and power requirements** increase cost.
- **Channel distortion** can alter the signal.
- SNR is used to describe noise performance.

---

# UNIT 2 — Signal Analysis and Transmission

## What is a Signal?
A signal is a function of one or more variables that carries information (e.g. telephone, radio, computer signals).

## ⭐ Energy vs. Power Signals
| | Energy Signal | Power Signal |
|---|---|---|
| Formula | Eg = ∫ g²(t) dt (finite) | Pg = lim(T→∞) (1/T)∫ g²(t) dt (finite) |
| When used | Signal amplitude → 0 as \|t\|→∞ (e.g. decaying exponential) | Signal does NOT die out (e.g. everlasting sinusoid, step) |
| Rule | If energy is finite → use energy as the size measure | If energy is infinite → use average power instead |

## ⭐ Full Signal Classification Table
| Category | Definition |
|---|---|
| **Continuous-time** | Defined at every instant of time |
| **Discrete-time** | Defined only at discrete instants |
| **Analog** | Amplitude can take any value in a continuous range |
| **Digital** | Amplitude has only a finite number of levels |
| **Deterministic** | No uncertainty — exactly described by a formula |
| **Non-deterministic / Random** | Uncertain value at some instant — described statistically |
| **Even signal** | x(t) = x(−t) |
| **Odd signal** | x(t) = −x(−t) |
| **Periodic** | x(t) = x(t + T) for some period T |
| **Aperiodic** | Never repeats |
| **Real signal** | x(t) = x*(t) (its own complex conjugate) |
| **Imaginary signal** | x(t) = −x*(t) |

*Example: x(t) = 3 → x\*(t) = 3 → real signal. x(t) = 3j → x\*(t) = −3j = −x(t) → imaginary(odd) signal.*

## Singularity Functions
| Function | Definition |
|---|---|
| **Unit step u(t)** | 0 for t<0, 1 for t>0 |
| **Unit impulse δ(t)** | 0 everywhere except t=0; ∫δ(t)dt = 1 (area = 1) |
| **Unit ramp r(t)** | 0 for t≤0, increases linearly (= t) for t>0 |

## Time Domain vs. Frequency Domain
- **Time domain**: signal plotted as amplitude vs time.
- **Frequency domain (line spectrum)**: signal plotted as amplitude/phase vs frequency.
- **Double-sided spectrum**: shown for both +f and −f (needed for real signals since real signal spectra are symmetric).

## ⭐ Worked Example — Plotting a Line Spectrum (exact style of 2024 exam Q3a)
**Problem:** Sketch the line spectrum of g(t) = 3 − 5cos(40πt − 30°) + 4sin(120πt)

**Step 1 — Rewrite every term as a positive cosine:**
g(t) = 3 + 5cos(40πt + 150°) + 4cos(120πt − 90°)
*(−cos θ = cos(θ+180°); sin θ = cos(θ−90°))*

**Step 2 — Build a table:**
| Term | Amplitude | Frequency (f = ω/2π) | Phase |
|---|---|---|---|
| DC | 3 V | 0 Hz | 0° |
| 40πt term | 5 V | 20 Hz | 150° |
| 120πt term | 4 V | 60 Hz | −90° |

**Step 3:** Plot each amplitude and phase as a vertical line at its frequency → this is the line spectrum.

## Sinc / Interpolating Function
sinc(x) = sin(x)/x for x≠0, and sinc(0)=1. It's an even function, equals 0 whenever x = ±π, ±2π, ±3π… Used to reconstruct/interpolate a continuous signal from samples.

## Fourier Series (for periodic signals)
Tells you: (1) how many frequency components exist, (2) their amplitudes, (3) their relative phases.

## ⭐ Fourier Transform (FT) — for non-periodic (everlasting) signals
**Forward:** X(ω) = ∫ x(t) e^(−jωt) dt  **Inverse:** x(t) = (1/2π) ∫ X(ω) e^(jωt) dω

### Key FT Pairs to memorize
| Signal x(t) | Transform X(ω) |
|---|---|
| δ(t) (unit impulse) | 1 |
| 1 (constant, all time) | 2πδ(ω) |
| cos(ω₀t) (everlasting sinusoid) | π[δ(ω−ω₀) + δ(ω+ω₀)] — two impulses at ±ω₀ |
| e^(−at)u(t), a>0 | 1/(a+jω) → magnitude = 1/√(a²+ω²) |
| rect(t/τ) (rectangular pulse of width τ) | τ·sinc(ωτ/2) |

## LTI Systems & Distortionless Transmission
For a linear time-invariant (LTI) system: **Y(f) = H(f)·X(f)**, and |Y(f)| = \|H(f)\|·\|X(f)\|, with phases adding.

**⭐ Condition for distortionless transmission:**
- Output = y(t) = k·x(t − t_d) (just a scaled, delayed copy — no shape change)
- **|H(f)| = k** (constant magnitude — flat gain over the band)
- **θ(f) = −2πf·t_d** (phase must be linear with frequency)
- **Group delay: t_d = −(1/2π)(dθ/df)**

## ⭐ Signal Distortion Over a Channel (asked in 2023)
| Type | What happens | Consequence |
|---|---|---|
| **Linear distortion** | Channel's H(f) isn't flat/linear-phase → causes magnitude and/or phase distortion. No new frequencies are created | Signal shape changes but stays within the same band; fixable with equalization |
| **Non-linear distortion** | y(t) = f(g(t)) = a₀+a₁g(t)+a₂g²(t)+... — the channel behaves non-linearly | **Creates brand-new frequency components** (harmonics) not in the original signal. If input BW = B, output BW becomes kB. **If two signals sit in adjacent bands, these new frequency components can spill into and interfere with the neighboring signal's band** |
| **Multipath distortion** | Signal arrives via multiple paths/delays; causes **frequency-selective fading** | Some frequencies fade more than others; combated using **Automatic Gain Control (AGC)** since fading varies with time |

## Energy/Power Spectral Density & Autocorrelation
- **Energy Spectral Density (ESD):** Ψg(f) = \|G(f)\|² — tells you how a signal's energy is distributed over frequency.
- **Power Spectral Density (PSD):** Sg(f) — same idea for power signals. Pg = ∫Sg(f)df
- **Autocorrelation** Rg(τ) = ∫g(t)g(t−τ)dt : measures how similar a signal is to a **delayed copy of itself**.
  - Rg(τ) and the ESD/PSD form a **Fourier Transform pair**.
  - **⭐ Why we need autocorrelation:** Random/noise-like signals (e.g. random binary data) **cannot** be described by a mathematical formula, so we can't take their Fourier Transform directly. But we CAN compute their autocorrelation, and then take *its* Fourier Transform to get the PSD.

---

# UNIT 3 — Amplitude Modulation (AM)

## Basic AM
- **Modulating/message signal**: contains the information, e.g. m(t) or x(t)
- **Carrier**: high-frequency sinusoid, e.g. cos(ω_c t)
- **⭐ AM wave equation:** s(t) = x(t)cos(ω_c t) + A cos(ω_c t) = [x(t) + A]cos(ω_c t) = E(t)cos(ω_c t), where E(t) = envelope
- DSB-SC (no carrier term): does not carry the discrete carrier, so it's called **Double SideBand Suppressed Carrier**.

## ⭐ Modulation Index (m)
**m = Em / Ec = (E_max − E_min) / (E_max + E_min)**
(Em = amplitude of modulating signal, Ec = amplitude of carrier)

## ⭐ Linear vs Over Modulation
| m value | Type | Result |
|---|---|---|
| m ≤ 1 (≤100%) | **Linear/normal modulation** | Envelope faithfully follows the message |
| m > 1 (>100%) | **Over-modulation** | **Envelope distortion** — the receiver can't recover the true message shape; should be avoided |

**Evaluating m in practice:** (1) directly from the AM signal itself, or (2) from a **trapezoidal display** on an oscilloscope.

## ⭐ Power & Transmission Efficiency of AM
For s(t) = x(t)cos(ω_c t) + Acos(ω_c t):
- **Carrier power:** Pc = A²/2
- **Sideband power:** Ps = (1/2)·x̄²(t) (x̄² = mean square/average of message)
- **Total power: Pt = ½[A² + x̄²(t)]**
- **⭐ Transmission efficiency: η = x̄²(t) / [A² + x̄²(t)] × 100%**
- For single-tone AM this simplifies to: **η = m² / (2+m²) × 100%**
- **Maximum possible efficiency = 33.33%** (occurs at m=1) — meaning at best, only 1/3 of transmitted power actually carries the message; the rest is "wasted" on the carrier.

### ⭐ Worked Example
*400 W carrier, modulated to a depth of 75% (m = 0.75). Find total power and efficiency.*
- Pt = 400×(1 + 0.75²/2) = 400×1.28125 = **512.5 W**
- η = 0.75²/(2+0.75²) × 100% = 0.5625/2.5625 × 100% = **21.95%**

## ⭐ Types of AM Modulators
| Type | How it works |
|---|---|
| **Multiplier modulator** | Variable-gain amplifier controlled by m(t); hard to keep linear, expensive |
| **Non-linear (square-law) modulator** | Uses a device with output y=ax+bx²; combining two such branches cancels unwanted terms, leaving DSB-SC |
| **Switching modulators (diode-bridge)** | A switch (diode bridge) toggles at the carrier rate, multiplying m(t) by a switching waveform |
| **⭐ Ring modulator** | 4-diode bridge circuit — a **double-balanced** switching modulator: on the carrier's positive half-cycle diodes D1,D3 conduct (output ∝ +m(t)); on the negative half-cycle D2,D4 conduct (output ∝ −m(t)). Output = (4/π)[m(t)cosω_ct − ⅓m(t)cos3ω_ct + ⅕m(t)cos5ω_ct − ...] |

## ⭐ Demodulation of DSB-SC / AM
| Method | How it works | When used |
|---|---|---|
| **⭐ Synchronous/Coherent detection** | Multiply received signal by a **locally-generated carrier** cos(ω_ct) in exact phase sync, then low-pass filter: e(t)=x(t)cos²(ω_ct) = ½x(t) + ½x(t)cos(2ω_ct) → LPF leaves ½x(t) | Needed for **DSB-SC/SSB** (no carrier present) — harder to build since local oscillator must match phase exactly |
| **⭐ Envelope/Non-coherent detection** | A simple rectifier + RC low-pass filter follows the envelope. Requires 1/f_c ≪ RC ≪ 1/B | Works only when there IS a strong carrier present, i.e. **0 ≤ m ≤ 1** — simplest option |

**⭐ Which detector for 0 ≤ m ≤ 1?** → **Non-coherent (envelope) detector**, because the carrier is present and the envelope always stays positive and clearly traces m(t) — no need for a complex synchronized local oscillator.

## Bandwidth-Efficient AM Variants
| Type | Method | Bandwidth |
|---|---|---|
| **Full AM (DSB+C)** | Both sidebands + carrier | 2B |
| **DSB-SC** | Both sidebands, no carrier | 2B |
| **⭐ SSB** | Remove one sideband using **Hilbert transform** (phase shift) | B (half of DSB) |
| **QAM** | Send 2 different messages on the same carrier frequency using quadrature (90°-shifted) carriers | 2B (but carries 2 messages) |
| **VSB** | Partially suppress one sideband (used in TV — easier to generate than SSB) | Slightly more than B |

## ⭐ Comparison Table: DSBFC vs DSBSC vs SSB vs VSB
| Parameter | DSBFC | DSBSC | SSB | VSB |
|---|---|---|---|---|
| Carrier suppression | None | Full | Full | None |
| Sideband suppression | None | None | One SB fully removed | One SB partially removed |
| Bandwidth | 2f_m | 2f_m | f_m | between f_m and 2f_m |
| Transmission efficiency | Minimum | Moderate | Maximum | Moderate |
| Modulating inputs | 1 | 1 | 1 | 2 |
| Application | Radio broadcasting | Radio broadcasting | Point-to-point mobile comm. | TV |

## Applications of AM
Radio broadcasting (AM), TV broadcasting, telephones.

---

# UNIT 4 — Angle Modulation (FM & PM)

## ⭐ What is Angle Modulation? FM vs PM
- **FM (Frequency Modulation):** carrier **frequency** varies with the modulating signal's amplitude; carrier amplitude stays constant.
- **PM (Phase Modulation):** carrier **phase** varies with the modulating signal's amplitude; carrier amplitude stays constant. PM is used to generate very stable FM.

## ⭐ Features / Advantages of Angle Modulation vs AM
- Provides better discrimination (robustness) against noise and interference than AM.
- This improvement comes **at the cost of increased transmission bandwidth**.
- Channel bandwidth can be traded for improved noise performance — **this trade-off is NOT possible with AM.**

## FM Signal Analysis
Message: v_m(t) = V_m cos(ω_m t). FM wave instantaneous frequency: ω_i = ω_c + K·v_m(t) = ω_c + KV_m cos(ω_m t)

Resulting FM wave: v_FM(t) = V_c cos[ω_c t + m_f sin(ω_m t)]

## ⭐ Frequency Deviation & Modulation Index
- **Frequency deviation: Δf = K·V_m** when K is given as ordinary frequency sensitivity (Hz/V or kHz/V).

> **Exam note:** The lecture also writes Δf = K·V_m/(2π) when K is defined as angular-frequency sensitivity. In the lecture/exam numerical example where K = 5 kHz and V_m = 2, use **Δf = K·V_m = 10 kHz**.
- **⭐ FM modulation index: m_f = Δf / f_m**

### ⭐ Worked Example (recurs almost exactly in past papers)
*Deviation sensitivity K = 5 kHz, modulating signal v_m(t) = 2cos(2π·2000t)*
- V_m = 2, f_m = 2000 Hz
- **Δf = K × V_m = 5000 × 2 = 10 kHz**
- **m_f = Δf/f_m = 10000/2000 = 5**

## ⭐ FM Bandwidth — Carson's Rule
- Theoretically FM needs infinite bandwidth, but practically, only a finite number of significant sidebands matter.
- **⭐ Carson's Rule: B = 2(Δf + f_m)**

### ⭐ Worked Example 2
*Frequency deviation = 10 kHz, modulating frequency = 10 kHz*
- B = 2(10+10) = **40 kHz**

---

# UNIT 5 — Sampling Theory & Pulse Code Modulation (PCM)

## Why Digital Pulse Modulation?
Most modern signals are digital; digital transmission reduces distortion and improves signal-to-noise performance. Types: **PAM, PWM, PPM (analog pulse), PCM & DM (digital pulse)**.

## ⭐ Analog Pulse Modulation Types
| Type | What varies | Advantages | Disadvantages | Applications |
|---|---|---|---|---|
| **PAM** | Pulse amplitude | Simple to build/demodulate | Needs large bandwidth, more noise | Ethernet, microcontroller control signals, photo-biology, LED drivers |
| **PWM (PTM)** | Pulse width | Low power, ~90% efficient, less noise interference | More complex circuit, voltage spikes, expensive | Telecom encoding, smart lighting brightness control |
| **PPM** | Pulse position | Best noise immunity of the three | Most complex, needs more bandwidth | Air-traffic control, remote-controlled vehicles, data compression/storage |

## ⭐ Digital Communication System (block diagram)
```
Message signal → Source encoder (Analog→Digital conversion happens here) → Baseband/line coding → Digital carrier modulation → Multiplexer → CHANNEL → Regenerative repeater → ... → Destination
```

## ⭐ Pulse Code Modulation (PCM)
**Transmitter:** Analog signal → LPF → **Sampler** → **Quantizer** → **Encoder** → PCM output
**Receiver:** (Regenerative repeater) → **Decoder** → Reconstruction filter → Destination

**Steps of digitization: Sampling → Quantization → (Binary) Coding**

### ⭐ Advantages / Disadvantages / Applications of PCM
| Advantages | Disadvantages | Applications |
|---|---|---|
| Used for long-distance comm.; better transmitter efficiency; higher noise immunity than analog | Needs more bandwidth; encoding/decoding/quantizing adds complexity | Satellite transmission, space communication, telephony, compact discs |

**Why PCM/digital makes it a "digital system":** because it converts a continuous analog signal into a discrete stream of binary digits (via sampling + quantizing + encoding), giving all the noise-immunity/multiplexing benefits of digital communication.

## ⭐ Sampling Theorem (Nyquist Criterion)
**The minimum sampling frequency needed to perfectly reconstruct a signal is: f_s ≥ 2f_m**
(f_s = sampling frequency, f_m = highest frequency component in the signal)

## ⭐ Aliasing & How to Avoid It
- **Aliasing** happens when f_s < 2f_m — the shifted spectral copies overlap, corrupting the signal (cannot be recovered correctly).
- **⭐ How to avoid aliasing:**
  1. Increase the sampling rate (f_s ≥ 2f_m)
  2. Use an **anti-aliasing filter** (a low-pass filter) before sampling to restrict the signal's bandwidth, so no energy exists above f_s/2.
  - Always apply the anti-aliasing filter **before down-sampling**.

## ⭐ Quantization
- **Quantization** = rounding each sampled value to the nearest one of a finite set of permissible levels — it reduces excess bits/data, but introduces **quantization error/noise**.
- **Uniform quantization**: step size is the same across the whole signal range (types: mid-tread, mid-rise).
- **Non-uniform quantization**: step size varies (finer steps for small signals) — classified separately, but the technique of achieving it (companding) is **not detailed in the provided lecture slides**.

## ⭐ Quantization Noise & Signal-to-Quantization-Noise Ratio (SQR)
- Quantization noise variance: **σ² = S²/12** (S = step size)
- **⭐ SQR formula: SQR (dB) = 1.76 + 6.02n** (n = number of bits per sample)
- **Key takeaway: every extra bit improves SQR by about 6 dB.**

| n (bits) | M (levels) | SQR (dB) |
|---|---|---|
| 2 | 4 | 13.8 |
| 4 | 16 | 25.8 |
| 6 | 64 | 37.8 |
| 7 | 128 | 43.8 |
| 8 | 256 | 49.8 |
| 9 | 512 | 55.8 |
| 10 | 1024 | 61.8 |

## Binary PCM (concept)
A sampled value is rounded to the nearest quantization level, then that level is assigned a **binary code number**. Example flow: Sample value → Quantized value → Code number → Binary code (e.g. a value of 1.3 might round to quantized level 1, code number 5, binary code `101`).

---
---

# ⚠️ Exam Topics NOT Covered in the Provided Lecture Slides
These appeared in the previous-year papers but the 5 lecture slide decks you gave me don't actually explain them (only the course-outline slide *names* some of these) — you'll need a textbook/other source for these specific items:
- **Line coding (NRZ, RZ formats)** — how to construct the waveform for a bit sequence like `11011010`
- **Non-uniform quantization technique using a uniform quantizer (companding)** — only the *name* is mentioned in the slides, not the method
- **White noise and Thermal noise** — only named in the course outline, never explained
- *(Correlation is partly covered — see Autocorrelation in Unit 2 — but no separate simple "correlation with example" is given.)*

---
---

# 1️⃣ MOST IMPORTANT TOPICS (highest study priority overall)
1. AM modulation index, over-modulation, and transmission efficiency (with numeric problems)
2. FM: frequency deviation, modulation index, and Carson's Rule bandwidth (with numeric problems)
3. Coherent vs. non-coherent detection and DSB-SC/SSB demodulation
4. Fourier Transform of standard signals (cos, e^(−at)u(t), impulse, rect) + plotting line spectra
5. Sampling theorem, aliasing, and anti-aliasing
6. Quantization noise and the SQR formula (1.76 + 6.02n)
7. Signal classification (energy/power, deterministic/random, even/odd, analog/digital)
8. Block diagram of a communication system + channel types & properties
9. Distortion types (linear, non-linear, multipath) and their consequences
10. Modulation basics: what it is, why it's needed, antenna-height example

# 2️⃣ TOPICS ASKED IN PREVIOUS YEARS (2023 & 2024 papers)
| Topic | Year(s) asked |
|---|---|
| Coherent vs non-coherent detector (m between 0–1) | 2023 & 2024 (identical question) |
| AM transmission/power efficiency (formula + numeric) | 2023 & 2024 |
| FM deviation, modulation index, Carson's Rule (numeric) | 2023 & 2024 |
| Modulation index & over/linear modulation | 2023 & 2024 |
| Fourier transform/series of a given signal + spectrum sketch | 2023 & 2024 |
| Simplex / half-duplex / full-duplex | 2023 & 2024 |
| Define modulating signal, carrier, AM envelope | 2023 & 2024 |
| Application of AM | 2023 & 2024 |
| Sampling theorem & aliasing | 2024 |
| Quantization & SQR derivation | 2024 |
| DSB-SC generation, spectrum, USB/LSB, rectifier demodulation | 2023 |
| Ring modulator / switching modulator (short note) | 2023 |
| Energy vs power signal (define/determine) | 2023 & 2024 |
| Analog vs digital, deterministic vs random, unit step/impulse | 2023 |
| Channel characteristics & most useful channel | 2023 |
| Optical fiber features | 2024 |
| Block diagram of communication system | 2024 |
| Linear/non-linear distortion & multipath effects | 2023 |
| Compare analog vs digital modulation | 2023 |
| PCM features, why PCM makes a system digital | 2023 |
| Non-uniform quantization technique (⚠️ not on slides) | 2023 |
| Angle modulation definition & features | 2023 |
| Why modulation reduces antenna height | 2023 |
| SSB phase-shift method | 2024 |
| Synchronous detection method | 2024 |
| Correlation, with example (⚠️ only autocorrelation covered) | 2024 |
| Line coding & NRZ/RZ construction (⚠️ not on slides) | 2024 |
| PAM definition & drawbacks | 2024 |
| White noise, thermal noise, SNR (⚠️ not on slides) | 2023 |

# 3️⃣ QUICK LAST-MINUTE REVISION NOTES
- **m = Em/Ec = (Emax−Emin)/(Emax+Emin)** — m≤1 normal, m>1 over-modulation (envelope distortion)
- **Pt = Pc(1+m²/2)**, **η = m²/(2+m²)×100%**, max η = 33.3% at m=1
- **Δf = K·Vm** (for K given in Hz/V or kHz/V), **mf = Δf/fm**, **Carson's Rule: B = 2(Δf+fm)**
- **fs ≥ 2fm** (Nyquist) → below this = aliasing → fix with anti-aliasing LPF
- **SQR(dB) = 1.76 + 6.02n** → +1 bit ≈ +6 dB
- Envelope detector → use when carrier present, 0≤m≤1 (simple). Synchronous detector → use when no carrier (DSB-SC/SSB), needs local oscillator in phase sync.
- **FT pairs:** δ(t)↔1, cos(ω₀t)↔π[δ(ω−ω₀)+δ(ω+ω₀)], e^(−at)u(t)↔1/(a+jω)
- **Distortionless transmission needs:** |H(f)| = constant AND phase linear with f
- Non-linear distortion → creates new harmonic frequencies → can leak into neighboring bands
- Multipath distortion → frequency-selective fading → fixed with AGC
- Simplex = 1-way | Half-duplex = 2-way, not together | Full-duplex = 2-way, together
- Antenna height = λ/4 = c/4f → higher carrier frequency = shorter practical antenna
- DSB-SC/SSB/VSB bandwidths: DSB=2fm, SSB=fm, VSB is in between
- PCM chain: Sample → Quantize → Encode (Tx) / Decode → Reconstruct (Rx)
- PAM = amplitude varies, PWM = width varies, PPM = position varies (PPM = best noise immunity)


---

# Class-Note Integration Note

The original class note was checked page-by-page against this resource. The final version above incorporates the useful additional material that was not already present, including the basic modulator block diagram, channel-selection factors, guided-media comparison, additional digital-communication points, AM applications, the explicit DSB-SC worked pattern, the square-wave Fourier-series form, angle-modulation applications, the full PCM system flow, and the class-note companding flow.

Where the class note conflicts with the uploaded lecture notation, the lecture-based formulation is retained and the discrepancy is kept explicit rather than silently changing the mathematics.
