---
title: Audio Fingerprinting
---

**Audio fingerprinting** is the method used by *Shazam* and *YouTube* to identify music from sound. AI methods are also being heavily used for audio fingerprinting and search these days

1. Compression using *Huffman Coding*. Lossless compression isn't necessary
2. Hashing ← Compact result, can be quickly searched and sorted with *log(N)* complexity
3. Audio fingerprinting is converting audio into a compact and quick-searchable format to compare and identify audio
4. Convert to raw .wav file and check for *sample rate*, *bit depth*, *number of channels*
5. Produce a *spectrogram* 
6. Encode the spectrograms for image search. Using locally sensitive hashing and the *Jaccard coefficient* as a distance metric

### Sources
- [How audio fingerprinting works?](https://emysound.com/blog/open-source/2020/06/12/how-audio-fingerprinting-works.html)
- [An Industrial-Strength Audio Search Algorithm - Shazam](https://www.ee.columbia.edu/~dpwe/papers/Wang03-shazam.pdf)
