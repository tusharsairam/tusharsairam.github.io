---
title: Otautahi-Oxford Model
---

Source: https://arxiv.org/abs/2502.16741
- **ISOs (Interstellar Objects)** give insights into galactic events and processes far beyond our solar system. ==That makes them important to identify and analyze==
- Such processes affect the *stellar velocity distribution* in the Solar neighbourhood. ==This also affects ISOs==. The **solar neighbourhood** is the region within the Milky Way that is close to the sun (~1000 light years). 
- They're not easily visible when between stars. However, their *substructure* becomes detectable only when they're within the Solar System's observable volume (solar neighbourhood). ==Important to note that a **substructure** in this context refers to the patterns of velocity distribution of the ISOs, and not their material composition==. Analogy: Bulk movement and behaviour of vehicles in traffic
- For added context on how important this paper is, ==3I/ATLAS is only the 3rd ISO detected by astronomers after 1I/'Oumuamua (2017) and 2I/Borisov (2019)==. It has been 6 years!
- $m_{r}$ is the **r-band apparent magnitude** of an object. Higher number => dimmer brightness (scales logarithmically). The r-band is ~620 nm (red light). This is important here because most present sky surveys like ATLAS and ZTF have *detection thresholds of* $m_r \sim 21.0-21.7$. Most ISOs are *pretty dim* and tough to detect with these. LSST however bumps up $m_{r}$ to 24.0 making detection of fainter ISOs more possible
- Normally, Gaussian velocity distributions were assumed for ISOs, and this was a simplification of an older time. However, when Gaia modeled the stellar kinematics in insane detail later, the realization was that ==ISOs followed a highly non-Gaussian and clumpy distribution==. *This necessitated the need for alternative and better models to predict their movements*
- A range of **Luminosity functions** must be considered for studying ISOs given that they can originate from different source systems. They follow a **Size-Frequency Distribution (SFD)**. ==A steeper SFD implies many small objects==, which means the ISOs are faint and small.  

* ==Large heliocentric volume needed to account for **grav. focusing**==. That's why slower ISOs veer closer towards the Sun
* Marceta's orbit-sampling method is *inefficent for non-zero-length surveys*. One would have to sample a much larger sphere and then perform orbit propagation, where most ISOs won't even enter the sphere
* **Residence time** is the amount of time during which an ISO stays in orbit inside the observable sphere
* **Peculiar velocity** is the velocity of a celestial object w.r.t a standardized frame (CMB, Local Standard of Rest etc.). In this case, the Sun's peculiar velocity needs to be considered in order to prevent systemic errors in the ISO orbit sampling procedure later on (why? because the sun itself moves!)

$F$ => ISO flux passing through a patch of sky. This depends on the following orbital parameters
* $B$ - Impact parameter
* $T$ - Survey time
* $\varphi$ - Position angle
* $(l, b)$ - Longitude, Latitude
* $v_{\infty}$ - Speed at infinity
* $n$ - Spatial number density (how many ISOs / unit volume)

- The authors make a few assumptions for orbit sampling
	- Small possibility of ISOs never completing their solar system passage is neglected
	- All ISOs follow perfectly hyperbolic orbits and non-gravitational accelerations are ignored
	- Isotropic flux is assumed
## Questions and Doubts
- [ ] `P(l, b, v_inf)` is the velocity distribution of ISOs in the solar neighbourhood. However, ChatGPT says it's the joint probability distribution for incoming direction
- [ ] Why is Marceta's sampling method inefficient? 
- [ ] When $T$ is a lot greater than the residence time, the ISOs already in the sphere are negligible to those that come in during the survey. I didn't understand this completely. If the influx of ISOs is that high, then the total count of ISOs already in the sphere grows proportionally too, so how does that become negligible? *Understanding this unlocks knowing the volume-sampling weighted distribution*
- [ ] Why is an isotropic flux assumed? I believe it's for computational simplicity but I want to check whether my explanation matches the paper's intentions
