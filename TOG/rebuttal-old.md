We thank reviewers for your valuable comments. Allow us to address the main points, followed by detailed questions.

We would like to emphasize that the main contribution of this work is devising a human-visual-system-matched real-time 6DoF neural synthesis method (***suitable for not only virtual environments but also physical captures/spaces, compatible with view-dependent effects***) for future low-storage and wearable VR/AR where only the pre-trained scene models need to be stored. 
The research addresses the high-latency(~xxx ms)/low-resolution(~xxx) problem in current neural synthesis, and the limited expressiveness (such as view-dependent lighting/illumination effects) in recent image (panorama or multi-layer image)-based representation/reprojection methods .

# Shared
## OURS vs. alternatives (why not panorama synthesis or multi-layer image, multi-sphere image representation)  (R2/R4/R2.4)

We will include a visual comparison with panorama+disocclusion method.
The method does support view-dependent effect, similar to NeRF. In the current demo, we didn't include illumination for faster data preparation. We will include visual examples demonstrating it.
Comparing with existing neural synthesis, it already reaches significantly higher resolution.

Add: Ln267
<!--Qi: OURS with a nerf style rendering supports correct illumination with different camera views, and we will promise to show photorealistic results during minor revision cycle. We need more points beyond this though.-->
<!--ZH: Need to explain NeRF for R2.6
   (R2: why do you need to run a ray-caster with a neural network at each sample location to achieve this style of foveated rendering? The method does not support view-dependent effects, the range of motion seems very limited, and the output resolution is quite low.)-->

## Constraints (r1.6, r2.5, r2.6, r4.4, r4.7):
<!--   (r1) restricted position and direction, and gaze motions?
   (r2) low (angular) resolution // head volume? frequency of transmiting network?
   (r4) occlusion (motion parallel): not clearly demonstrated-->
   
* Gaze and head rotation 

Our gaze range covers the entire display-supported FoV (100deg). The rotational range is only determined by the computing resources and duration for training. A larger rotation means a larger training set (longer training time).  In the "gallery" scene, we trained for up to 240deg rotational range. We are confident to include a 360 rotational result within a minor revision cycle.

* Translation (head volume)

In our experiments, we tested $30cm^3$. Ln1068-1070/Fig.11 depict a limitation: developing a multi scale network that synthesizes the image from global to local level-of-details would be an interesting direction for future work, extending the applicability of our method to large-scale scenes with strong occlusions. We support motion parallax and occlusion within a certain range. Unlike the pure rotation method, we do not need to train a network for every viewpoint.
In the demo scenes<!--Qi: point to the exact time range-->, there are few close objects, so the motion parallax is not significantly demonstrated. We will include larger scale translation and rotation range within a minor revision cycle. 0.5MB is not for "one view", but a local "range" as visualized in Fig.11.

* Angular resolution

Our angular resolution (6 PPD) is a trade-off between performance (determined by computational resource) and quality (determined by display capability). 
and yet as verified by user-study, users cannot distinguish us from the ground truth (10+ PDD). However, this already significantly outperforms existing neural synthesis solutions. 
<!--Qi: We are asked about "WHY" the 6PPD appears the same as 16 PPD even in fovea, not repeating the content already-->

## Mobile devices (r1.1, r2.2):
<!--   
(r1) experiment is done on high-end machine but paper is targeting for mobile device?

ZH: We mentioned 'mobile' only once, in the introduction L160. Shall we try and report a number on mobile VR device?

(r2) On what hardware is this method running in real-time
ZH: CPU is Intel(R) Xeon(R) Gold 6230R CPU@2.10GHz (256GB RAM) and one NVIDIA GTX 3090 graphics card.

(r2) distance towards low latency, and low-storage VR/AR platforms?-->

We target at future wearable VR/AR devices that are "akin to" (Ln160) mobile devices whose low storage and computational power demands dynamically fetching content from cloud. Our method enables fast fetching with only 0.5mb so that the system can keep running without any network-induced latency. Since it allows local translation, the method can fetch (for future positions) while rendering in parallel. Further, on mobile devices, the supported resolution (PPD) is lower, demanding low inference computation.

<!--Qi: @Nianchen, the best if you could quickly try to run that on a mobile phone or having collaborators do that asap-->


## Static scene encoding (r1.2, r1.3, r3.5, R4):
<!--   (r1) no user interaction, mobile devices can store
   (r3) scope should be clarified-->
We focus on encoding/decoding static scene in this project. 
Encoding dynamic scenes and interaction support are orthogonal challenges that are getting attention to the neural rendering community:
"D-NeRF: Neural Radiance Fields for Dynamic Scenes", arxiv 2020.
We thank R3 for the suggestion of clarifying the scope. We will tone down and clarify this limitation for static scene and future intention to match the model with dynamic scenes and temporal visual perception such as saccades (R4).


   <!--Accept r3’s suggestion and express scope more clearly in later revisions-->
   <!--ZH: we should probably write down "a clearer scope" directly instead of claim we 'will'. (R3: the main goal of the present work is to focus on bringing the rich static environments in VR with efficient rendering 
   or this work considers only optimizing the decoding/rendering part of the problem, while the encoding is still an open research problem and the submission explicitly leaves it out of the scope.)-->

<!--## About encoding performance (r3.5, r4.3):
   (r3) scope should be clarified
   (r4) viewing is instant, but creation is not (encoding/training is slow)-->
   This research assumes that the encoding of the scene (network training) is executed in a large-scale cloud service only once (which is practically available), so the computing overhead is ignored. <!--This is not the scope of this article. 
   Accept r3’s suggestion and express scope more clearly in later revisions (ZH: better pick one or write a version of scope)-->

## User study (r1.4, r3.4, r4.6): (R1/R3/R4)
   <!--(r1, r4) short, gaze fixed / static
   (r1) leave no time to experience ego-motion parallax effects
   (r3) narrow foveation methods selection-->
   <!--DNC: The content of the user study is arranged in this way to minimize the interference of other non-core factors on the experimental results (such as the performance defect of the eye tracker, the VR novice is not familiar with the equipment and the environment, etc.) 
   The focus of this article is not to study how to provide more accurate foveation rendering, so the most typical method is selected as the ground truth-->
The user study is simulating how user experience different conditions. 
Similar to similar studies [REF], the short duration is guarantees fairness among trials (avoid shifting LnXXXX).
We asked users to fixate at certain gaze positions so everyone's views were consistent across conditions and participants. (TBD) We set duration to 300ms so it is close to saccade experience(?)

<!-- 3/9
1) 300ms > per frame
2) even not enough, already proved ours > nerf

motion parallax is exactly our beneifts than panorama synthesis we will compare with panorama
it is fair for each view

-->
   
With eye-tracking in VR, foveated rendering is chose as GT because that has been shown to be perceptually identical to a full resolution rendering. Higher resolution on the peripheral range has minimal effects on perception. We have done an orthogonal pilot study verifying the identicality between F-GT and GT. We will include the details. <!--(TODO: read the claim of the 2002 paper.)-->


## Publish source code (r3.3, r5.1):
Yes, we will make the entire source code publicly open, including models and data.
<!--
About written (r3.1, r3.2, r4.14, r5.6):
Qi: just ignore these
   (r3) improve clarity / bib consistency
   (r4) typo / style
   (r5) data flow figures; 
   (r5) what is the overall loss function, how was the training done (batches? training dataset?) ZH: we can try best to increase R5's score by just claiming we will add whatever [s]he thought is helpful.
-->
## Individual
Review 1


<!--Qi: what is this parapgraph for?-->
0.5MB is the upper bound of the storage space we need (or the storage space we need is unrelated with the complexity and scale of the scene), and the storage space required for traditional 3D resources is closely related to the fineness of the model/texture (no upper bound)* 

* Foveation rendering as ground truth (r1.5)

Review 2
* in angular resolution (you need fewer pixels per sphere, but also fewer spheres), this amounts to (6/10)^3 *10 = 0.06MB per frame for the maximum 6PPD resolution used in this paper.

* Angular resolution: 6.4 PPD vs ground truth 13 PPD? (r2.1)
  <!--DNC: the users are asked to choose an overall perceptually better quality, not the fovea region only. as LPIPS shown, we have better quality in mid and far periphery, 
  ZH: the above explanation is not even clear to me.
  ZH: We are using F-GT (foveated ground truth images) instead of high resolution ground truth.
  (ZH: I am confused again about the reason we choose F-GT, does that mean our final retina images has passed two foveated rendering here?)
* Experiment about storage? (r2.3)
    What baseline requires 100MB of storage claimed in the introduction? The original 3D asset? There must be a middle ground which improves storage somewhat, without having to evaluate a neural network for every output pixel.
* Other multi-sphere representation (r2.4)
    In particular, I would also like to see how a multi-sphere image representation like [Broxton2020] or [Attal2020] would perform. For example, in [Broxton2020] they report 10MB per frame for 10PPD data. Since storage is cubic in angular resolution (you need fewer pixels per sphere, but also fewer spheres), this amounts to (6/10)^3 *10 = 0.06MB per frame for the maximum 6PPD resolution used in this paper.
Review 3 (None)

Review 4
* not comparable between neural and mesh representation (r4.2)
* ZH: 0.5MB storage for our model is not only for one view. (more to go here).
* ZH: 0.5MB is the upper bound of the storage space we need. The storage space we need is unrelated to the complexity and scale of the scene, however the storage space required for traditional 3D resources is closely relevant to the fineness of the model/texture, which has no upper bound.
* A fairer comparison: allow NeRF an additional 96 channels (r4.8)
   ZH: We are using the same parameters that were used in our setup. Nevertheless, the performance of such NeRF configuration is still very slow. 
  <!--DNC: Same network parameters means same network capacity. Use large network capacity to compare our small network capacity is unfair.-->
* Why our is better? foveat network: still represents high-res whole scene, why baseline is worse? (4.9)
  <!--DNC: We use different coordinates (Egocentric coordinates) while baseline uses 3D volume. We are for different tasks. 3D volume is not suitable for our inside-out view mode.-->
* Artifacts in video:
  1m30: double image artifacts (r4.5)
  1m03: black splotches, where they come from? (r4.10)
  ZH: we have black holes in mid-periphery (why?)
* Minor questions:
  *Misunderstanding: need depth reconstruction for 360 photos application. (r4.1)
  <!--DNC: The neural encoding the scene information, including the depth. So we do not need extra depth information in run-time. Depth information can be inferred by ray casting using output density-->
  Not understand Fig 7 (r4.11)
  <!--DNC: The bounding refers to the range of sampling in ray casting (the near and far plane), which affects the distribution of sampling distances. we set upper bound to 10000 in (a) to approximate the infinite because sampling are done in disparity space (1/distance)-->
  Isn't the representation just alpha composited front to back? (r4.12)
  <!--DNC: Ray cast is different from alpha blending. it is an integration of density. Refer to NeRF for the detail equation-->
  Is NeRF here also running on a TensorRT implementation? (r4.13)
  <!--DNC: No-->
  ZH: explain the potential effects here.

Review 5
* Hard to read (Not understand)
  "The entire experiment of each user consists of 36 trials, 6 trials per pair of ordered conditions."
  --> how did this come about? 3 pairs of conditions x 2 scenes? (r5.2)
  ZH: 6 trials (2 scenes x 3 gaze positions) x 6 pairs (permutation of 2 conditions). We predefined 3 gaze positions for each scene. Participants were asked to fixate each predefined gaze position for each trial and made the choice.  
  "Each condition from an individual scene consists of 3 different gaze positions."
  --> I did not understand what this meant. Once participants had moved their eyes 3 times, the experiment stopped? (r5.3)
  "LPIPS uses deep neural networks to estimate perceptual similarities between the image provided and a reference image."
  --> What is meant by perceptual similarity? color similarity? (r5.4)
ZH: Perceptual similarity is not only color similarity. It indicates the similarity of human visual perception.IPIPS (TBD)
  "We used one-way repeated measures ANOVAs to compare effects across three stimuli on each eccentricity range and together."
  --> What is being treated as repeated measures? what is the independent and dependent variable? and what does "together" refer to? (r5.5)
ZH: IV: three conditions (F-GT, OURS, NeRF). DV: LPIPS value at eccentricity=5, 10, 15, ... ,105.


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
6. suggestion of writing/figures.

