# Communication Engineering — Final Merged Exam Notes

---

# UNIT 1 — Introduction to Communication Systems

## What is Communication?
Communication is the process of establishing a connection between two points to exchange information (voice, text, images, data, etc.).

## ⭐ Block Diagram of a Communication System
```
Information source → Input transducer → Transmitter (Modulator) → CHANNEL (+Noise) → Receiver (Demodulator) → Output transducer → Destination
```
| Block | Function | Example |
|---|---|---|
| Information source | Produces the message — not electrical yet | Voice, music, pictures, text, image, video |
| Input transducer | Converts the physical message into a time-varying **electrical** signal | Microphone, camera, scanner |
| Transmitter | Restricts frequency range, amplifies, and **modulates** — superimposes the message onto a high-frequency carrier | Amplification, Modulation (Analog: AM/FM/PM; **Digital: ASK/FSK/PSK/DM**) |
| Channel | The medium the signal travels through. Noise gets added here (noise is always random in character) | Twisted-pair cable, coaxial cable, optical fiber |
| Receiver | Extracts the original message from the degraded received signal | Amplification, Demodulation |
| Output transducer | Converts the electrical signal back into its original physical form | Loudspeaker converts electrical signal into sound |
| Destination | Final user/device the information is intended for | Computer, mobile phone |

> **Note:** Digital modulation has its own family, parallel to AM/FM/PM: **ASK (Amplitude-Shift Keying), FSK (Frequency-Shift Keying), PSK (Phase-Shift Keying), and DM (Delta Modulation)**.

## Bandwidth
**Bandwidth = range of frequencies a signal occupies = f₂ − f₁**
> Example: music signal spans 20 Hz–15 kHz → BW = 15000 − 20 = **14,980 Hz**. Typical bandwidths: Speech 300–3100 Hz | Music ~15 kHz | Video 0–5 MHz

## ⭐ Communication Channels — Properties & Comparison
A channel is judged on: power needed for the desired S/N ratio, bandwidth, amplitude/phase response, linear/non-linear behaviour, external interference.

| Channel | Key facts | Advantages | Disadvantages |
|---|---|---|---|
| **Telephone (twisted pair)** | Bandpass ≈300–3400 Hz, SNR ≈30 dB, nearly linear | Cheap | Limited bandwidth |
| **Coaxial cable** | 50Ω/75Ω types; carries 10,000+ voice channels; backbone of analog telephone networks | Noise-immune (shielded), large BW, low loss | Pricier than twisted pair (cheaper than fiber) |
| **Optical fiber** | Thin glass/plastic core carrying light | Small & light, large BW, **no EM interference**, long distance | High initial/maintenance cost, jointing/testing difficulty, fiber losses |
| **Terrestrial microwave** | Needs line-of-sight; unidirectional | No cables needed, wide BW, multiple channels | Weakens from multipath reception; can't penetrate walls |
| **Satellite** | Relay station; uplink 6GHz/downlink 4GHz (lecture example); geostationary height ≈35,683 km | Covers huge areas | Very high build/launch cost |

→ **Optical fiber is usually the "most advantageous" channel** (huge bandwidth + immune to interference), despite the higher cost.

### ⭐ Why are twisted-pair cables twisted?
Twisting the two wires together (rather than running them parallel) **reduces electromagnetic interference (EMI) and minimizes crosstalk** between adjacent wires.
- Reduces EMI
- Minimizes crosstalk
- Improves signal quality
- Supports higher data rates

### Twisted Pair vs Coaxial vs Optical Fiber — quick comparison
| Feature | Twisted Pair | Coaxial | Optical Fiber |
|---|---|---|---|
| Transmission medium | Copper wires twisted together | Copper conductor with shielding | Glass and plastic fiber |
| Signal type | Electrical | Electrical | Light |
| Bandwidth | Low–medium | Medium–high | Very high |
| Data speed | Low–medium | High | Very high |
| Distance | Short | Medium | Long |
| Cost | Low | Medium | High |

## ⭐ Classification of Communication Systems
**1) By direction**
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
| Bandwidth needed | Less | Large |
| Accuracy | Continuous (no quantization error) | Involves quantization → approximation |
| Noise immunity | Low | High |
| Security | Low | High |
| Multiplexing | Only **FDM** | **FDM, TDM, and CDM** |
| Design | More difficult | Easily designed |
| Advantages | Simple Tx/Rx, low bandwidth | Reliable, noise-resistant, easy multiplexing/encryption, **easy error detection & correction**, **easy storage and processing**, **supports multimedia**, better security |
| Drawbacks | Noise degrades signal and can't be separated out; no repeaters between Tx/Rx | Needs more bandwidth for high bit rates; needs synchronization; **more complex equipment**; **higher initial cost**; **higher power consumption**; sampling error possible |

**3) By transmission technique:** Baseband (signal sent as-is) vs Modulated transmission (message carried on a high-frequency carrier).

## ⭐ What is Modulation & Why Do We Need It?
**Modulation** = varying a property (amplitude, frequency, or phase) of a high-frequency **carrier** according to the **modulating (message) signal**.

**Why we need it (5 reasons):**
1. Reduces antenna height
2. Avoids mixing/interference of signals from different sources
3. Increases the range of communication
4. Enables multiplexing (many signals sharing one channel)
5. Improves quality of reception

### ⭐ Worked Example — why modulation reduces antenna height
Minimum practical antenna height ≈ **λ/4 = c/(4f)**
- f = 10 kHz → h = (3×10⁸)/(4×10×10³) = **7,500 m (7.5 km)** → impossible to build
- f = 1 MHz → h = (3×10⁸)/(4×10×10⁶) = **75 m** → practical
- f = 10 MHz → h = (3×10⁸)/(4×10×10⁶... ×10) = **7.5 m** → even smaller

### ⭐ Why must the carrier frequency be higher than the modulating-signal frequency?
- Efficient radiation
- Long-distance transmission
- Smaller antenna size
- Avoids mixing of signals
- Easy reception

### Basic block diagram of a modulator
```
Baseband signal ──┐
                   ├──► [ Modulator ] ──► Modulated signal ──► [ RF Power Amplifier ] ──► to antenna
     Carrier ───────┘         ▲
                               │
                        [ RF Oscillator ]
```

## Types of Modulation (quick overview)
| Type | What varies | Notes |
|---|---|---|
| AM | Carrier amplitude | Analog |
| FM | Carrier frequency | Analog |
| PM | Carrier phase | Analog; used to generate stable FM |
| PAM / PWM / PPM | Pulse amplitude / width / position | Pulse-based (analog pulse modulation) |
| ASK / FSK / PSK | Amplitude/Frequency/Phase Shift Keying | Digital modulation counterparts of AM/FM/PM |
| PCM / DM | — | Digital pulse modulation |

## Limitations of Communication Systems
- **Noise and interference**
- **Limited bandwidth**
- **Transmission delay**
- **Security risks**
- **Equipment cost**
- **Power requirement**
- **Channel distortion**

---

# UNIT 2 — Signal Analysis and Transmission

## What is a Signal?
A signal is a function of one or more variables that carries information.

## ⭐ Energy vs. Power Signals
| | Energy Signal | Power Signal |
|---|---|---|
| Formula | Eg = ∫g²(t)dt (finite) | Pg = lim(T→∞)(1/T)∫g²(t)dt (finite) |
| When used | Amplitude → 0 as \|t\|→∞ | Signal does NOT die out |
| Rule | Finite energy → use energy | Infinite energy → use average power |

> **Lecture example:** Eg = ∫₋₁⁰2²dt + ∫₀^∞4e⁻ᵗdt = 4+4 = **8**. For a sawtooth-type signal, Pg = ∫₀¹t²dt = **1/3**.

## ⭐ Full Signal Classification Table
| Category | Definition |
|---|---|
| Continuous-time | Defined at every instant of time |
| Discrete-time | Defined only at discrete instants |
| Analog | Amplitude can take any value in a continuous range |
| Digital | Amplitude has only a finite number of levels |
| Deterministic | No uncertainty — value at any time can be predicted exactly by a formula, e.g. x(t)=5sin(2πft) |
| Non-deterministic / Random | Future value cannot be predicted exactly |
| Even signal | x(t) = x(−t) |
| Odd signal | x(t) = −x(−t) |
| Periodic | x(t) = x(t+T) |
| Aperiodic | Never repeats |
| Real signal | x(t) = x*(t) |
| Imaginary signal | x(t) = −x*(t) |

## Singularity Functions
| Function | Definition |
|---|---|
| Unit step u(t) | 0 for t<0, 1 for t>0 |
| Unit impulse δ(t) | 0 everywhere except t=0; ∫δ(t)dt=1. Key FT pair: **δ(t) ↔ 1** |
| Unit ramp r(t) | 0 for t≤0, equals t for t>0 |

### ⭐ Worked Example — energy vs power classification
For x(t) = e⁻ᵃᵗu(t), a>0: E = ∫₀^∞e⁻²ᵃᵗdt = 1/2a < ∞, P=0 → **Energy signal**
For x(t) = u(t): E = ∫₀^∞1dt = ∞, P = ∫₋T/2^T/2 1 dt/T = 1/2 (finite, nonzero) → **Power signal**
For x(t) = 1/t: both E and P diverge → **Neither energy nor power signal**

## Time Domain vs. Frequency Domain
Time domain: amplitude vs time. Frequency domain (line spectrum): amplitude/phase vs frequency. A double-sided spectrum shows both +f and −f.

## ⭐ Worked Example — Plotting a Line Spectrum (2024 exam Q3a style)
**Problem:** Sketch the line spectrum of g(t) = 3 − 5cos(40πt − 30°) + 4sin(120πt)

**Step 1 — rewrite every term as a positive cosine:**
g(t) = 3 + 5cos(40πt + 150°) + 4cos(120πt − 90°)

**Step 2 — table:**
| Term | Amplitude | Frequency (f=ω/2π) | Phase |
|---|---|---|---|
| DC | 3 V | 0 Hz | 0° |
| 40πt term | 5 V | 20 Hz | 150° |
| 120πt term | 4 V | 60 Hz | −90° |

**Step 3:** plot each amplitude & phase as a vertical line at its frequency.

## Sinc / Interpolating Function
sinc(x) = sin(x)/x for x≠0, sinc(0)=1. Even function; zero at x=±π,±2π,±3π… Used to reconstruct/interpolate a continuous signal from samples.

## Fourier Series (periodic signals)
Tells you: (1) which frequency components exist, (2) their amplitudes, (3) their relative phases.

### Link between Fourier Series and Fourier Transform
A Fourier series only works for a *periodic* signal (period T₀). If you stretch that period out — T₀ → ∞ — the spacing between spectral lines (ω₀ = 2π/T₀) shrinks toward zero, so the discrete line spectrum merges into a **continuous** spectrum. That continuous-spectrum result *is* the Fourier Transform. In short: **FT is what a Fourier series becomes when the period is stretched to infinity** (i.e. the signal is no longer periodic).

### ⭐ Worked Example — Fourier series of a square wave
For a square wave with period T=2 (so ω₀=π), amplitude ±1:
a₀ = 0, aₙ = 0 (odd function → sine series only)
**f(t) = (4/π) Σ (1/n)sin(nπt)**, n = 1,3,5,… (odd only)
- Amplitude spectrum: Aₙ = A/(nπ) for n=1,3,5,… → A₁=4/π, A₃=4/3π, A₅=4/5π,…; Aₙ=0 for even n
- Phase spectrum: odd harmonics → φₙ = −90°; even harmonics → not present

## ⭐ Fourier Transform (FT) — for non-periodic (everlasting) signals
**Forward:** X(ω) = ∫x(t)e⁻ʲωᵗdt   **Inverse:** x(t) = (1/2π)∫X(ω)eʲωᵗdω

### Key FT pairs to memorize
| Signal x(t) | Transform X(ω) |
|---|---|
| δ(t) (unit impulse) | 1 |
| 1 (constant, all time) | 2πδ(ω) |
| cos(ω₀t) (everlasting sinusoid) | π[δ(ω−ω₀) + δ(ω+ω₀)] |
| e⁻ᵃᵗu(t), a>0 | 1/(a+jω) → magnitude 1/√(a²+ω²), phase −tan⁻¹(ω/a) |
| rect(t/τ) (rectangular pulse, width τ) | τ·sinc(ωτ/2) |

## Modulation Viewed in the Frequency Domain
Multiplying a message signal by a carrier **shifts its spectrum** up to the carrier frequency — this is the frequency-domain reason modulation works at all.

For φAM(t) = m(t)cos(ωct), with m(t) ↔ M(f):
**φAM(t) ↔ ½[M(f+fc) + M(f−fc)]** — the message spectrum M(f) gets copied to sit centered at +fc and −fc.

**Demodulation reverses this:** multiplying by cos(ωct) again,
φAM(t)cos²(ωct) = ½m(t)[1 + cos(2ωct)]
— this puts a copy of M(f) back at baseband (f=0) plus a copy shifted out to ±2fc. A low-pass filter keeps only the baseband copy, recovering ½m(t).

> *(This is the general worked pattern behind "Example: find the FT of x(t)=rect(t/4)cos(10t)" — a pulse cos(10t) has a spectrum that is the pulse's own spectrum shifted to ±10 rad/s.)*

> ⚠️ **Out of syllabus this year (your teacher said to skip Lecture-02 from slide 30 onward):** LTI systems & distortionless transmission, linear/non-linear/multipath distortion, energy & power spectral density, and autocorrelation. If this changes, ask and I'll add them back.

---

# UNIT 3 — Amplitude Modulation (AM)

## Basic AM
**⭐ General AM wave:** s(t) = [A + x(t)]cos(ωct) = x(t)cos(ωct) + Acos(ωct)   Envelope: E(t) = A + x(t)

## Single-Tone AM
Let x(t) = Vmcos(ωmt). Using cosα·cosβ = ½[cos(α+β)+cos(α−β)]:
s(t) = Acos(ωct) + (Vm/2)[cos(ωc+ωm)t + cos(ωc−ωm)t]
```
        LSB      Carrier      USB
         |          |          |
      fc−fm         fc       fc+fm
```
If message bandwidth = B: **BWAM = 2B**

## DSB-SC
**DSB-SC (Double Sideband Suppressed Carrier)**: contains both sidebands but **no discrete carrier**. AM = Carrier+USB+LSB; DSB-SC = USB+LSB only. **BWDSB-SC = 2B**

### ⭐ Worked Example — full DSB-SC problem (2023 Q4b exact style)
*For baseband signal m(t) = sin(ωmt), find the DSB-SC signal, sketch its spectrum, identify USB/LSB, and verify that DSB-SC can be demodulated by a rectifier detector.*

**① DSB-SC signal:** carrier c(t) = cos(ωct)
s(t) = m(t)c(t) = sin(ωmt)cos(ωct) = **½[sin(ωc+ωm)t + sin(ωc−ωm)t]**
- ωUSB = ωc + ωm
- ωLSB = ωc − ωm

Spectrum: two lines of amplitude ½ at frequencies (ωc−ωm) [LSB] and (ωc+ωm) [USB], symmetric about ωc.

**② Rectifier demodulation check:** |s(t)| = |sin(ωmt)cos(ωct)| — rectifying (taking the absolute value / envelope) recovers a waveform whose slowly-varying envelope traces |sin(ωmt)|, confirming DSB-SC can be recovered this way (in practice, followed by a low-pass filter to smooth it into m(t)).

## ⭐ Modulation Index (m)
**m = Em/Ec = (Emax−Emin)/(Emax+Emin)**   (trapezoidal display: m = (A−B)/(A+B))

### Derivation (envelope method)
Em = (Emax−Emin)/2 and Ec = (Emax+Emin)/2 → m = Em/Ec = **(Emax−Emin)/(Emax+Emin)** ∎

## ⭐ Linear vs Over Modulation
| m value | Type | Result |
|---|---|---|
| m < 1 | Under/linear modulation | Message amplitude smaller than carrier; AM wave transmitted without distortion |
| m = 1 | 100% modulation | Message amplitude equals carrier amplitude; maximum modulation without distortion |
| m > 1 (>100%) | **Over-modulation** | Message amplitude greater than carrier; **envelope distortion** — avoid this |

> **Practice example** *(generic — not from the lecture slides, for practicing the formula)*: Given Emax=9V, Emin=3V → m = (9−3)/(9+3) = 6/12 = **0.5** → 50% modulation

## ⭐ Which detector for 0 ≤ m ≤ 1?
For a normal AM signal wave (0≤m≤1), use a **non-coherent (envelope) detector** — because m≤1 means the AM envelope never touches zero, so it can be recovered without needing carrier synchronization. For DSB-SC (no carrier present), coherent/synchronous detection is required instead.

## ⭐ Power & Transmission Efficiency of AM
Carrier power: **Pc = A²/2**   Sideband power: **Ps = ½x̄²(t)**   Total power: **PAM = ½[A² + x̄²(t)]**; for single-tone AM, **Pt = Pc(1+m²/2)**
Transmission efficiency: **η = (PUSB+PLSB)/Pt = x̄²(t)/[A²+x̄²(t)] × 100%**; for single-tone AM, **η = m²/(2+m²) × 100%**
**Maximum possible efficiency = 33.33%** (at m=1).

### ⭐ Worked Example (recurs in both 2023 & 2024 papers)
400 W carrier, modulated to a depth of 75% (m=0.75):
Pt = 400×(1+0.75²/2) = 400×1.28125 = **512.5 W**
η = 0.75²/(2+0.75²) × 100% = 0.5625/2.5625 × 100% = **21.95%**

## ⭐ Types of AM Modulators
| Type | How it works |
|---|---|
| Multiplier modulator | Variable-gain amplifier controlled by m(t); hard to keep linear, expensive |
| Non-linear (square-law) modulator | Device with output y=ax+bx²; combining two branches cancels unwanted terms, leaving DSB-SC |
| **Switching modulator** | Carrier signal acts as a switching signal; a transistor or diode is used as the switch — simple and efficient. Output: **w(t)m(t) = ½m(t) + (2/π)[m(t)cosωct − ⅓m(t)cos3ωct + ⅕m(t)cos5ωct − ...]** |
| **⭐ Ring modulator** | Balanced modulator using **4 diodes** — good carrier suppression, output contains upper & lower sidebands, double-balanced. Positive half-cycle: D1,D3 conduct, output ∝ +m(t). Negative half-cycle: D2,D4 conduct, output ∝ −m(t). Output: **w(t)m(t) = (4/π)[m(t)cosωct − ⅓m(t)cos3ωct + ⅕m(t)cos5ωct − ...]** |

## ⭐ AM Demodulation
| Method | How it works | When used |
|---|---|---|
| **Synchronous/Coherent detection** | Multiply by locally-generated carrier cos(ωct) in exact phase sync, then LPF: e(t)=x(t)cos²(ωct)=½x(t)+½x(t)cos(2ωct) → LPF leaves ½x(t) | Needed for **DSB-SC/SSB** (no carrier present) |
| **Envelope/Non-coherent detection** | Rectifier + RC low-pass. Requires 1/fc ≪ RC < 1/B | Works when carrier IS present, **0≤m≤1** |

## Bandwidth-Efficient AM Variants
| Type | Method | Bandwidth |
|---|---|---|
| Full AM (DSB+C) | Both sidebands + carrier | 2B |
| DSB-SC | Both sidebands, no carrier | 2B |
| **SSB** | Remove one sideband via **Hilbert transform** | B |
| QAM | Two messages on quadrature carriers | 2B (carries 2 messages) |
| VSB | Partially suppress one sideband, used in TV | Slightly > B (TV example: SSB=4.5MHz, VSB=6MHz, DSB=9MHz) |

## ⭐ Comparison Tables
**AM vs FM vs PM**
| Feature | AM | FM | PM |
|---|---|---|---|
| Varying quantity | Amplitude | Frequency | Phase |
| Carrier amplitude | Varies | Constant | Constant |
| Applications | Radio/TV broadcast | TV sound, FM radio, police wireless | Used to generate stable FM |

**Coherent vs Envelope Detection**
| Feature | Coherent/Synchronous | Envelope |
|---|---|---|
| Method | Multiply by local carrier, then LPF | Rectify + RC low-pass |
| Carrier requirement | Correct local carrier needed (phase-synced) | No sync needed |
| Used for | DSB-SC / SSB | Ordinary AM (0≤m≤1) |

**DSBFC vs DSBSC vs SSB vs VSB**
| Parameter | DSBFC | DSBSC | SSB | VSB |
|---|---|---|---|---|
| Carrier suppression | None | Full | Full | None |
| Sideband suppression | None | None | One SB fully removed | One SB partially removed |
| Bandwidth | 2fm | 2fm | fm | between fm and 2fm |
| Transmission efficiency | Minimum | Moderate | Maximum | Moderate |
| Modulating inputs | 1 | 1 | 1 | 2 |
| Application | Radio broadcasting | Radio broadcasting | Point-to-point mobile comm. | TV |

**PAM vs PWM vs PPM**
| Scheme | Changes | Key advantage |
|---|---|---|
| PAM | Amplitude | Simple modulation/demodulation |
| PWM | Width | Lower power, ~90% efficient, less noise |
| PPM | Position | Constant amplitude, best noise immunity |

## Applications of AM
Radio broadcasting, aircraft communication, shortwave radio communication, two-way radio communication, television broadcasting, remote control systems, emergency communication.

---

# UNIT 4 — Angle Modulation (FM & PM)

## ⭐ What is Angle Modulation? FM vs PM
- **FM:** carrier **frequency** varies with message amplitude; carrier amplitude constant.
- **PM:** carrier **phase** varies with message amplitude; carrier amplitude constant. Used to generate stable FM.

## ⭐ Features / Advantages of Angle Modulation vs AM
- Better discrimination (robustness) against noise and interference than AM.
- Comes at the cost of increased transmission bandwidth.
- Channel bandwidth can be traded for improved noise performance — **not possible with AM**.

## FM Signal Analysis
Message: vm(t) = Vmcos(ωmt). Instantaneous frequency: ωi = ωc + K·vm(t)
Resulting FM wave: vFM(t) = Vccos[ωct + mfsin(ωmt)]

> **⭐ Frequency Deviation & Modulation Index — use this formula:**
> **Δf = K·Vm** (K already in Hz/V or kHz/V — no ÷2π)
> **mf = Δf / fm**

### ⭐ Worked Example 1 (recurs almost exactly in past papers)
K = 5 kHz/V, vm(t) = 2cos(2π·2000t) → Vm=2, fm=2000 Hz
**Δf = K×Vm = 5×2 = 10 kHz**   **mf = Δf/fm = 10000/2000 = 5**

## ⭐ FM Bandwidth — Carson's Rule
**B = 2(Δf + fm)**

### ⭐ Worked Example 2 — Carson's Rule
Δf = 10 kHz, fm = 10 kHz → B = 2(10+10) = **40 kHz**

### ⭐ Worked Example 3 — full FM phase-form problem (2024 Q3c exact style)
*Find the carrier frequency, modulating frequency, modulation index, and maximum deviation for v = 10sin(5×10⁸t + 6sin1350t)*

Compare with the general form v = Asin(ωct + β·sin(ωmt)):
- A = 10 V
- ωc = 5×10⁸ rad/s
- ωm = 1350 rad/s
- β (modulation index) = 6

**① Carrier frequency:** fc = ωc/2π = (5×10⁸)/(2π) = **7.96×10⁷ Hz** (79.6 MHz)
**② Modulating frequency:** fm = ωm/2π = 1350/(2π) = **214.85 Hz**
**③ Modulation index:** mf = β = **6**
**④ Maximum (peak) frequency deviation:** Δf = mf×fm = 6×214.85 = **1289.15 Hz**

---

# UNIT 5 — Sampling Theory & Pulse Code Modulation (PCM)

## Why Digital Pulse Modulation?
Types: **PAM, PWM, PPM** (analog pulse), **PCM, DM** (digital pulse).

## ⭐ Analog Pulse Modulation Types
| Type | What varies | Advantages | Disadvantages | Applications |
|---|---|---|---|---|
| PAM | Pulse amplitude | Simple modulation/demodulation, easy Tx/Rx construction | Large bandwidth, more noise, more power needed | Ethernet, microcontroller control signals, photo-biology, LED drivers |
| PWM | Pulse width | Low power, ~90% efficient, less noise interference, high power-handling | Complex circuit, voltage spikes, expensive, switching loss | Telecom encoding, DC-motor speed control, smart lighting |
| PPM | Pulse position | Constant amplitude, best noise immunity, good power efficiency | Highly complex, needs more bandwidth | Air-traffic control, remote-controlled vehicles, data compression/storage |

## ⭐ Pulse Code Modulation (PCM)
```
Transmitter: Analog message → LPF → Sampler → Quantizer → Encoder → (Regenerative repeater) →
Channel →
Receiver: (Regenerative repeater) → Regenerative circuit → Decoder → Reconstruction filter → Destination
```
**Four steps: ① Sampling** (take samples of the analog signal) **② Quantization** (convert samples into fixed levels) **③ Encoding** (convert levels into binary bits) **④ Decoding** (convert binary bits back into levels, at the receiver)

| Advantages | Disadvantages | Applications |
|---|---|---|
| Long-distance comm.; better transmitter efficiency; higher noise immunity than analog | Needs more bandwidth; encoding/decoding/quantizing adds complexity | Satellite transmission, space communication, telephony, compact discs |

### ⭐ How is PCM different from other systems? What makes it "digital"?
- PCM converts an analog signal into digital binary pulses (0s and 1s).
- Other modulation systems (AM, FM, PM) use a continuous carrier signal.
- PCM uses sampling, quantization, and encoding.
- PCM has better noise immunity.
- **A system is called "digital"** when information is represented using discrete values (usually binary 0 and 1), **rather than continuous values.**

## ⭐ Sampling Theorem (Nyquist Criterion)
**fs ≥ 2fm**
| Condition | Result |
|---|---|
| fs > 2fm | Spectra remain separated |
| fs = 2fm | Nyquist limit |
| fs < 2fm | **Aliasing occurs** |

## ⭐ Aliasing & How to Avoid It
1. Increase the sampling rate (fs ≥ 2fm)
2. Use an **anti-aliasing filter** before sampling — always before down-sampling

> **Worked Example:** if fm=5kHz, minimum fs ≥ **10 kHz**. Any value below causes aliasing.

## ⭐ Quantization
Quantization = rounding each sample to the nearest of a finite set of permissible levels — reduces excessive bits/data but introduces **quantization error/noise**.
Step size: **S = (VH−VL)/M**   Error: e=V−Vq, −S/2≤e≤S/2   Noise power: **σ² = S²/12**

**Uniform quantizer**: same step size throughout. **Non-uniform quantizer**: varies step size (finer for small signals).

### ⭐ Achieving non-uniform quantization using a uniform quantizer (companding)
*(2023 Q8a — previously flagged as not covered; now added from your friend's notes, verified correct)*
```
Input signal → Compressor → Uniform Quantizer → Encoder → Transmission →
Decoder → Expander → Output signal
```
The **compressor** squeezes the signal's dynamic range before quantizing it with an ordinary uniform quantizer — this effectively gives small-amplitude parts of the signal finer resolution and large-amplitude parts coarser resolution, i.e. non-uniform quantization. At the receiver, the **expander** undoes the compression to restore the original signal range. (This compressor+expander pair is called **companding**.)

## ⭐ Signal-to-Quantization-Noise Ratio (SQR)
**SQR (dB) = 1.76 + 6.02n**   (n = bits/sample) — every extra bit improves SQR by ~6 dB
| n (bits) | M (levels) | SQR (dB) |
|---|---|---|
| 2 | 4 | 13.8 |
| 4 | 16 | 25.8 |
| 6 | 64 | 37.8 |
| 7 | 128 | 43.8 |
| 8 | 256 | 49.8 |
| 9 | 512 | 55.8 |
| 10 | 1024 | 61.8 |

> **Worked Example:** 8-bit PCM → SQR = 1.76+6.02(8) = **49.92 dB ≈ 49.8 dB**

## Binary PCM (concept)
```
Sample value → Quantized value → Code number → Binary code
```

---

# Ready-to-Write Exam Definitions
| Term | Definition |
|---|---|
| Modulation | The process of varying one or more properties of a high-frequency carrier according to the information-bearing modulating signal. |
| AM | The process of varying the amplitude of a high-frequency carrier in accordance with the instantaneous amplitude of the information signal, while carrier frequency and phase remain constant. |
| FM | The process in which the carrier frequency varies according to the amplitude of the modulating signal, while carrier amplitude remains constant. |
| Angle modulation | Modulation in which the frequency or phase of the carrier is varied according to the message signal. |
| SSB | An AM technique in which either the upper or lower sideband is removed, giving a bandwidth of B for a message of bandwidth B. |
| PCM | A method of converting an analog signal into digital form by sampling, quantization, and binary encoding. |
| Quantization | The process of representing each sampled value by the nearest value from a finite set of permissible levels. |
| Aliasing | The error caused by spectral overlap when the sampling frequency is less than twice the highest frequency component of the signal. |
| Digital system | A system in which information is represented using discrete values (usually binary 0/1) rather than continuous values. |

---

# ⚠️ Exam Topics NOT Covered in the Provided Lecture Slides
| Topic | Status |
|---|---|
| Non-uniform quantization technique (companding) | ✅ **Now covered above** — added from your friend's notes |
| Line coding (NRZ, RZ formats) — constructing the waveform for a bit sequence like `11011010` | ❌ Still not covered — need a textbook/other source |
| White noise and Thermal noise (detailed explanation) | ❌ Still not covered — only named in the course outline |
| Correlation, general definition with a simple example | ⚠️ Partly covered — see Autocorrelation in Unit 2 — but no separate simple "correlation with example" is given anywhere in your materials |

---
# 3️⃣ Quick Last-Minute Revision Notes
```
COMMUNICATION SYSTEM
Source → Transducer → Transmitter → Channel → Receiver → Destination
                                       ↑
                                     Noise

DUPLEX
Simplex: A→B | Half-duplex: A→B / A←B (one at a time) | Full-duplex: A→B / A←B (simultaneously)

CABLES
Twisted pair: low-med BW, cheap | Coaxial: med-high BW, shielded | Fiber: very high BW, light, no EMI

MODULATION
AM→amplitude | FM→frequency | PM→phase | Carrier must be higher freq than message (radiation, distance, antenna size)

AM
s(t) = [A+x(t)]cos(ωct)         BW = 2B
m = (Emax−Emin)/(Emax+Emin)     Pt = Pc(1+m²/2)     η = m²/(2+m²)×100%
m<1 → under/linear | m=1 → 100% | m>1 → over-modulation

DSB-SC:  USB+LSB, no carrier         SSB: one sideband only, BW=B
Synchronous detector: DSB-SC → ×local carrier → LPF → message
Envelope detector: used when 0≤m≤1 (carrier present)

FM
Δf = K·Vm      mf = Δf/fm      Carson's Rule: BW = 2(Δf+fm)

SAMPLING
fs ≥ 2fm          fs < 2fm → aliasing

PCM
Sampling → Quantization → Encoding → (Decoding at Rx)
S = (VH−VL)/M     e = V−Vq     σ² = S²/12
SQR = 1.76+6.02n dB     +1 bit → ~+6 dB SQR
Non-uniform quantization = Compressor → Uniform quantizer → ... → Expander (companding)

NOISE
SNR = Ps/Pn     SNR(dB) = 10log₁₀(Ps/Pn)
```

**Most repeated exam areas overall:** AM equation/spectrum, modulation index, AM power & efficiency, DSB-SC detection, Fourier analysis, energy/power signals, FM deviation & Carson's rule, sampling/aliasing, quantization/SQR/companding, PCM, communication-system basics, cable comparisons, and duplex systems.
