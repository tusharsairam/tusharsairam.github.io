---
title: Stopping Power - Radiation
---
>[!definition] Stopping Power
>The energy loss of particles per unit length through the target material. It's also called **Linear Energy Transfer (LET)**[^2] and its unit is *MeV-cm^2/mg*

I first learnt about this during my thesis at Excillum. The X-ray spectrometer I was working with had a Beryllium window to permit the X-rays through and a CdTe detector. The working principle of the detector was that the *slowdown or stoppage of X-ray photons in the detector material would be converted to electric charge* that can be read out to measure the emission spectrum of the X-ray source. That's where stopping power comes in. ==Higher stopping power -> more suitable for detection of stronger EM radiation==. Excillum deals with hard X-rays of high energys, so CdTe is a great material for that given its high stopping power[^1]

Stopping power depends on **particle mass**, **particle energy**, and **target material density**[^2]

>[!bug] Stopping power doesn't exactly mean what you think it is!
>I had a misconception that stopping power is a measure of *how well a material can slow down or stop impending radiation*. However, that's not the case. ==Stopping power specifically applies to particles, not radiation== given that definition. I thought Aluminium is selected to shield spacecraft components because it would have a high stopping power but that's wrong. *Aluminium has a low atomic number (Z = 13)*, so that makes it a poor radiation stopper actually. However, that also means it produces less bremsstrahlung radiation when radiation strikes it, which is good so you don't have a mini X-ray factory inside the component by accident. It can also block a decent portion of particles anyways and it's low cost, easy to machine, and widely available


[^1]: [Ametek - Si-PIN vs CdTe Comparison](https://www.amptek.com/internal-products/si-pin-vs-cdte-comparison)
[^2]: [LANL - Basic Mechanisms: Total Ionizing Dose](https://uspas.fnal.gov/materials/19NewMexico/Radiation/lecture_6.pdf)
