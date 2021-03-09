# Grouped and Organized
## Shared
1. About hardware requirement (r1.1, r2.2):
   (r1) experiment is done on high-end machine but paper is targeting for mobile device?
   (r2) distance towards low latency, and low-storage VR/AR platforms?
2. About static scene (r1.2, r1.3, r3.5):
   (r1) no interaction, mobile devices can store
   (r3) scope should be clarified
3. About encoding performance (r3.5, r4.3):
   (r3) scope should be clarified
   (r4) viewing is instant, but creation is not (encoding/training is slow)
4. About user study (r1.4, r3.4, r4.6):
   (r1, r4) short, gaze fixed / static
   (r3) narrow foveation methods selection
5. About constraint (r1.6, r2.5, r2.6, r4.4, r4.7):
   (r1) restricted position and direction?
   (r2) low (angular) resolution // head volume? frequency of transmiting network?
   (r4) occlusion (motion parallel): not clearly demonstrated
6. Publish source code (r3.3, r5.1):
   (r5) to help understanding
7. About written (r3.1, r3.2, r4.14):
   (r3) improve clarity / bib consistency
   (r4) typo / style

## Individual
Review 1
* Foveation rendering as ground truth (r1.5)

Review 2
* Concern about the comparison with MSI ([Broxton2020] or [Attal2020]):
  MSI can achieve same low storage under constraint of no view-dependent effects, small motion range, low resolution (r2.4, r2.6, r2.7)
* Angular resolution: 6.4 PPD vs ground truth 13 PPD? (r2.1)
* Experiment about storage? (r2.3)

Review 3 (None)

Review 4
* not comparable between nerual and mesh representation (r4.2)
* A fairer comparison: allow NeRF an additional 96 channels (r4.8)
* Why our is better? foveat network: still represents high-res whole scene, why baseline is worse? (4.9)
* Artifacts in video:
  1m30: double image artifacts (r4.5)
  1m03: black splotches, where they come from? (r4.10)
* Minor questions:
  Misunderstanding: need depth reconstruction for 360 photos application. (r4.1)
  Not understande Fig 7 (r4.11)
  Isn't the representation just alpha composited front to back? (r4.12)
  Is NeRF here also running on a TensorRT implementation? (r4.13)

Review 5
* Hard to read (Not understand)
  "The entire experiment of each user consists of 36 trials, 6 trials per pair of ordered conditions."
  --> how did this come about? 3 pairs of conditions x 2 scenes? (r5.2)
  "Each condition from an individual scene consists of 3 different gaze positions."
  --> I did not understand what this meant. Once participants had moved their eyes 3 times, the experiment stopped? (r5.3)
  "LPIPS uses deep neural networks to estimate perceptual similarities between the image provided and a reference image."
  --> What is meant by perceptual similarity? color similarity? (r5.4)
  "We used one-way repeated measures ANOVAs to compare effects across three stimuli on each eccentricity range and together."
  --> What is being treated as repeated measures? what is the independent and dependent variable? and what does "together" refer to? (r5.5)


# Raw Collections
## Review 1
1. Target mobile device but experiment on high-end machine?
2. Static scene: mobile devices can store. how about dynamic scene?
3. No interaction
4. user study: fix gaze, short
5. foveation rendering as ground truth
6. restricted position and direction?

## Review 2
1. Angular resolution: 6.4 PPD vs ground truth 13 PPD?
2. Performance: hardware? distance from low latency, and low-storage VR/AR platforms?
3. experiment about storage?
4. MSI: 10PPD - 10 MB -> 6PPD - 0.06MB?
5. head volume? frequency of transmiting network?
6. powerful computer but low resolution. MSI can achieve same storage real-time on mobile
7. why ray-caster? why not bake to MSI? (Under constraint of no view-dependent effects, small motion range, low resolution)

## Review 3
1. Improvement of clarity: the introduction of the egocentric coordinates might be a bit dense/slow to read
2. Reference: make bibliography more consistent
3. Publish source code
4. User study: the selection of foveation methods to compare is somewhat narrow
5. The scope should be clearly stated: static and receive side (decoder) only

## Review 4
1. Misunderstanding: 360 photos application: need depth reconstruction, e.g., Serrano et al. 2019 IEEE VR.
2. not comparable between nerual and mesh representation
3. Encode is slow, viewing is instant, but creation is not
4. Occlusion: small.
5. double image artifacts at 1m30s in video
6. Static user study: only a subset of experience
7. Translation and occlusion (motion parallax) are not clearly demonstrated and validated
8. A fairer comparison: allow NeRF an additional 96 channels
9. Why our is better? foveat network: still represents high-res whole scene, why baseline is worse?
10. Artifacts: 1m03 in video: black splotches, where they come from?
11. (Minor) Not understande Fig 7
12. (Minor) Isn't the representation just alpha composited front to back?
13. (Minor) Is NeRF here also running on a TensorRT implementation?
14. Typos/style:
    L154: "remarkably" <- can remove this; the reader can determine whether it is remarkable or not.
    L324: using 'd' for density here is not a huge problem but might be confused for 'depth'. Later on (L614) this is refered ot as 'a'.
    L1064: "unprecedented performance" <- please tone it down a bit; every technique is a trade-off somewhere.
    Duplicated citation of Attal et al.

## Review 5
1. Hard to read
2. "The entire experiment of each user consists of 36 trials, 6 trials per pair of ordered conditions." --> how did this come about? 3 pairs of conditions x 2 scenes?
3. "Each condition from an individual scene consists of 3 different gaze positions." --> I did not understand what this meant. Once participants had moved their eyes 3 times, the experiment stopped?
4. "LPIPS uses deep neural networks to estimate perceptual similarities between the image provided and a reference image." What is meant by perceptual similarity? color similarity?
5. "We used one-way repeated measures ANOVAs to compare effects across three stimuli on each eccentricity range and together." --> Unfortunately this is too concise an explanation to make sense -- what is being treated as repeated measures? what is the independent and dependent variable? and what does "together" refer to?

