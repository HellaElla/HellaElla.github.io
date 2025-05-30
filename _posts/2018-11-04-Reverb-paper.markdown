---
layout: post
title:  "Done With Grad School, so here's a paper about Reverb"
date:   2025-05-09
categories:
---

# About Grad School


Thesis defense was Thursday, went ok. 
Just submitted my last paper today, and I thought it was pretty casual and ok... decent and interesting enough to put here, so here goes!

## Room Simulation and Artificial Reverberation approaches within the context of Spatialized Audio Applications.


Artificial Reverberation has been available for over 50 years at this point, with the advent of techniques for generation of artificial reverb and room simulation via computational methods being introduced by Schroeder in 1961 via comb filtering and allpass filters. Reverberation refers to the persistence of sound after the direct sound from a source has stopped. Sound generally arrives to a listener in stages and, while in an enclosed environment provides listeners with a characterization of a space which plays a major part in room acoustics; a direct sound arrives at a listener at T0 and early reflections will follow from nearby surfaces to said direct sound. While both of these can provide cues for sound localization it is important to note that early reflections primarily convey information about the materials present in the space as well with an accumulation of later reflections presenting a listener with a tail that is indicative of the size of the space (Valimaki et al., 2012) RT60: the time it takes for a reverberant sound to decrease by 60 decibels after the original sound has stopped, as introduced by W.C Sabine.

Although previously, physical models of acoustic environments had been utilized as a tool for the design of acoustical environments and physical spaces used to artificially apply reverberation to signals in the 1920s their construction and need for physical space limited their use; later on computational methods proved to be a more efficient method. For auralization needs, approaches to artificial reverberation can be broken down into three main methodologies:

1. Delay Networks
2. Convolutional
3. Acoustical computation/ geometrical acoustics.


In this paper we will focus on Delay Networs and Convolutional Techniques.

### *Delay Networks and Schroeder:*

Artificial Reverberation using Delay Networks was first introduced in 1960 and utilized a series of allpass filters in combination with comb filters to produce a series of delays, spectral content and decay characteristics in order to approximate the diffuse nature of reverberation in a room, and is a foundational approach to artificial reverb (Schroeder & Logan, 1961). When early reflections are concerned Schroeder achieved these via parallel comb filters that introduce a series of delays at fixed intervals, each comb filter reinforces specific frequencies, creating resonant peaks in the response; by using multiple comb filters with slightly different delays, the response is smoothed and the resonances are spread out, avoiding “significant” coloration. Delay lengths in the comb are chosen specifically to be non-multiples of each other to avoid periodicity in the comb filter outputs, while gain parameters primarily affect the decay time of the reverb tail. The output of the parallel comb filters section is then processed through a series of all-pass
filters to increase the echo density and smooth out the tail.

Still, this approach has some limitations. Firstly, it is not physically accurate and relies primarily on a perceptual approach. Secondly, the “comb filter stage” of this design may introduce some unwanted coloration if not enough care is put into the implementation, and lastly it does not model directional sound propagation. In any case, this is a powerful approach that remains efficient, with Schroeder’s later work introducing an implementation with a more simplified FIR filter and Moore (1979) expanding on this to introduce a simple first-order low-pass filter within the feedback loop in the comb filter stage of the design to simulate air absorption.

### *Feedback Delay Networks and Binaural Applications:*

The use of impulse response-convolution methods can be a good approach for exact simulation if the acoustical properties of a room, however for dynamic applications parameterized simulation can provide increased flexibility for this context. Multichannel Feedback Delay methods have been introduced for the efficient and dynamic auralization of virtual rooms within the context of Binaural applications. Changing a FDN base stereo reverberation implementation involves inserting a binaural decoder within the path of the
architecture where the direction of the direct sound and respective early reflections can be simulated using Head Related Transfer Functions (Jot et al., 2025; Menzer, 2010).

FDN methods usually employ only a single network to simulate both early reflections and reverberation. However, these single delay line methods have been shown to be somewhat inaccurate when compared to a captured room impulse response, hence a parallel-delay-network based methodology was introduced. This methodology allows one to “produce good ambient reverb from the beginning of the impulse response and to use the other to produce distinct early reflections at the precise instants when they should occur according to the image source model”(Menzer, 2010, p. 2). 

According to Stautner and Puckette (1982) each channel of a FDN can be seen as the corresponding barrier where sound is reflected (i.e: the number of walls in a room; 4 when ceiling and floor are not considered), although they are perceptually relevant (the Menzer model can be expanded to include them by adding additional channels in a similar manner to the
2 dimensional case). When a sound source is considered, the position of reverberant signals stemming from the image source model correspond to the output of the reverberator channel, which is thus then convolved with the HRTF that is appropriate for the position given; frequency dependent dampening can be implemented as well using filters inside the loop. (Menzer, 2010)

### *A Note on Geometrical Acoustics*

Physically based approaches aim to approximate solutions for the wave equation, since solving the wave equation for audible ranges in large spaces is computationally intensive. The accuracy of these approximations heavily depends on the proposed application, for instance reverberation that is perceptually approximate might be sufficient for certain applications while some other tasks like architectural acoustics demand a higher level of accuracy. Additionally, some of these approaches can work for real time applications, while others generate an impulse response that are later used for convolution based reverb (Valimaki et al., 2012). Various approaches have been suggested in order to generate Binaural Room Impulses with a lighter computational load (Wendt, van de Par and Ewert, 2014). 

However, the focus here is not on geometrical acoustics.

### *Impulse Response Methods:*


Impulse responses in tandem with convolution have been a tested approach to room simulation, where the implementation of convolutional reverb is equivalent to the large FIR filtering of a signal, with zero delay convolutional approaches being currently widely used (Gardner, 1995),especially considering fast convolution based implementations where the convolution is performed in the frequency domain FFT methods in order to speed up computation. In short, measuring an impulse response via playing a broadband noise impulse, using a clapper, or if desired a sine sweep for greater accuracy in the capture (sine sweeps being the standard methodology for measured impulses) where the measured signal is then convolved with the reversed signal used during measurement to obtain the impulse response. Convolving a signal with the resulting impulse response will result in the output of the system that the impulse
response represents.

### *Using Binaural Room Impulse Responses:*

As seen above, a standard approach to simulating static binaural reverberation is by utilizing Binaural Room Impulse Responses. A simulated reverberant sound can be produced by convolution of the dry singal with the BRIR for both ears; BRIRs can be captured by Binaural microphones such as the Neumman KU-100 head; this is the most straightforward approach when Binaural Room Impulse Responses are concerned. When interactive movements are needed, the convolutional filter is formed by interpolating the BRIRs of neighboring listening points. However, some argue that there exist some limitations to capturing BRIRs (Wendt, van de Par and Ewert, 2014). Several approaches to simulating IRs have been proposed, where it is typical the an IIR can closely approximate a FIR filter with reduced computational cost, where RIRs are divided into subbands and modelled separately.

Interesting techniques with a hybrid approach have also been employed by Wendt, van de Par and Ewert (2014) in order to binaurally auralize a room where the early reflections are generated utilizing the image source method under the assumption that the simulated environment is a shoe-box room with low-order early reflections involved. 

The reverberant tail of a room is perceptually simulated using Feedback Delay Network methods allowing specific control of the reverberation characteristics. Spatialization in this case resembles the binaural application of FDN reverb outlined above, where the output of delay lines is convolved with corresponding number of spatially distributed HRTF locations; this system is suited for 6 degrees of freedom and can result in mostly good subjective ratings.

### *Closing Thoughts:*

While Artifcial Reverberation has been available for quite some time, ranging from physical-analog methods to computationally efficient implementations for simulating complex environments, it is clear that feedback delay networks remain foundational and offer good results with low computational cost that are perceptually sound, regardless of limitations in physical accuracy. By incorporating spatialization in FDN approaches trough the integration of Head Related Transfer Functions. Convolutional techniques can offer a more direct and accurate approach to simulated room acoustics, wether recorded or simulated, particularly when Binaural Room Impulse Responses are available for use. While their use might be limited in dynamic applications, advances in hybrid techniques have provided more options and flexibility for this use case. The distinction between these methods can reflect the trade offs between perceptual realism and physical accuracy.

### *References*

Chowning, J. M. (1977). The Simulation of Moving Sound Sources. Computer Music Journal,
1(3), 48. https://doi.org/10.2307/3679609

Gardner, W. G. (1995). Efficient Convolution without Input/Output Delay. Journal of the Audio
Engineering Society, 43(3), 127–136.

Jot, J.-M., Warusfel, O. W., Mein, M., & Khale, E. (2025). Jean-Marc Jot, Olivier Warusfel,
Eckhard Kahle, Mireille Mein: Binaural Concert Hall Simulation in Real Time (IEEE 93,
Mohonk (USA) 1993). IRCAM Multimedia LibraryMédiathèque de l’Ircam © Ircam,
1996-2008 - Research Institute in Acoustics and MusicInstitut de Recherche et
Coordination Acoustique/Musique - Paris, France. Ircam.fr.
http://architexte.ircam.fr/textes/Jot93a/index.html

Menzer, F. (2010). Binaural Reverberation Using Two Parallel Feedback Delay Networks. AES,
2.

Moorer, J. A. (1979). About This Reverberation Business. Computer Music Journal, 3(2), 13.
https://doi.org/10.2307/3680280

Schroeder, M. R., & Logan, B. F. (1961). “Colorless” artificial reverberation. IRE Transactions
on Audio, AU-9(6), 209–214. https://doi.org/10.1109/tau.1961.1166351

Stautner, J., & Puckette, M. (1982). Designing Multi-Channel Reverberators. Computer Music
Journal, 6(1), 52. https://doi.org/10.2307/3680358

Valimaki, V ., Parker, J. D., Savioja, L., Smith, J. O., & Abel, J. S. (2012). Fifty Years of
Artificial Reverberation. IEEE Transactions on Audio, Speech, and Language
Processing, 20(5), 1421–1448. https://doi.org/10.1109/tasl.2012.2189567
