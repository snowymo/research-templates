We thank the reviewers' valuable comments. Allow us to address the main points, followed by detailed questions.

We would like to emphasize that the main contribution of this work is devising a human-vision-matched (resolution and stereo acuity) 6DoF neural representation that can be inferred in real-time for VR near-eye displays. The method is applicable for not only virtual environments but also physical captures/spaces, compatible with view-dependent effects for future low-storage, wearable and cloud-based VR/AR where only the pre-trained scene models will need to be dynamically transmitted. The research addresses the high-latency<!--Qi: any nerf number for its resolution vs time consumption per frame?--> problem in current neural representation, and the limited expressiveness (such as correct lighting/illumination effects) in the recent image (panorama or multi-layer image)-based representation/re-projection methods.

# Necessity of neural representation method 
Q: Comparison with traditional mesh representation (R2/R4):

First, There is a misunderstanding about the applicable case of the work. Our (and NeRF's) goal is not about encoding mesh-based virtual scenes. The neural network is a representation of radiance field. That is, besides virtual scenes, physical scenes just follow exactly same computation procedure. The only data we need to train such a model are several RGB images with corresponding intrinsic and extrinsic parameters.
We apologize for not including such examples and will include new examples with both physical scenes and dynamic lighting effects to visually demonstrate the necessity.

Further, the network is not related to the complexity of scenes. The size of a mesh scene is related to its geometric and textural fineness, which may be extremely large (e.g. hundreds or even thousands MB) with full visibility (e.g., terrain), but the size of the network is a constant (0.5MB in our experiments). In the cloud-based application scenario that all data are acquired from cloud services at run-time, the data size is an important factor that affects the user experience.

Q: Comparison with MSI methods (R2):

First, our neural synthesis method naturally support correct dis-occlusions and motion parallax as the representation treats each view uniformly (inside the first sphere as in Fig. 11).
Meanwhile, for physical scenes, MSI methods (e.g. panorama-based pre-baking) require additional image-based inference of occlusions and depths. Ln267 mentions that these methods "typically suffers from trade-offs among performance, achievable resolution, and dis-occlusion effects due to lack of knowledge of the 3D space". Directly recording a precise multi-layer panorama requires sophisticated devices.

Second, similar to NeRF, the method does support view-dependent effect that is lacking with image-based panorama pre-baking solutions. In the current demo, we didn't include global illumination for faster data preparation. We will include visual examples demonstrating it.

To clearly demonstrate the difference, we will include a visual comparison with panorama+disocclusion method.

<!--Qi: OURS with a nerf style rendering supports correct illumination with different camera views, and we will promise to show photorealistic results during minor revision cycle. We need more points beyond this though.-->
<!--ZH: Need to explain NeRF for R2.6
   (R2: why do you need to run a ray-caster with a neural network at each sample location to achieve this style of foveated rendering? The method does not support view-dependent effects, the range of motion seems very limited, and the output resolution is quite low.)-->

# Limitations

<!-- (r1) restricted position and direction, and gaze motions?
     (r2) low (angular) resolution // head volume? frequency of transmiting network?
     (r4) occlusion (motion parallel): not clearly demonstrated-->

Q: The range of motion and parallax support (R1/R2/R4)

Our gaze range covers the entire display-supported FoV ($100^\circ$). The rotational range is only determined by the computing resources and duration for training. A larger rotation means a larger training set (longer training time).  In the "gallery" scene, we trained for up to $240^\circ$ rotational range. We are confident to include a $360^\circ$ rotational result within a minor revision cycle.
For head volume, in our experiments, we tested $30cm^3$. Ln1068-1070 and Fig.11 depict the balance: although computationally the innermost sphere (that defines the supported translation range) could any large, significantly enlarge it would cause visual quality decreasing.
Developing a multi scale network that synthesizes the image from global to local level-of-details would be an interesting direction for future work, extending the applicability of our method to large-scale scenes with strong occlusions. We support motion parallax and occlusion within the range determined by the innermost sphere. Unlike the pure rotation method, we do not need to train a network for every viewpoint.

<!--Q: Occlusion and Motion parallax(R2/R4)-->

We support correct occlusion and motion parallax at any view position in the range of motion. These visual effects are demonstrated, although not significantly visible, in the video (1m20s ~ 1m25s). We will include larger translation to demonstrated more significant occlusion and motion parallax within a minor revision cycle.

<!--Qi: (3/10) I think we can potentially drop this one since R2's point isn't really about the resolution but alternative solutions, right?-->

Q: The angular resolution (R2)

Our angular resolution (6.4 PPD) is a trade-off between performance (determined by computational resource) and quality (determined by display capability). 

<!--DNC: How to measure the actual PPD of VR? I've searched a value of 5.1'/pixel (about 11.8 PPD)-->

We do use full resolution ground truth images F-GT in the user study. 

<!--Qi: We are asked about "WHY" the 6PPD appears the same as 16 PPD even in fovea, not repeating the content already-->
<!--DNC: "Why" is hard to talk about because to some extent, this shows that the method we regard as ground truth is not the best foveation rendering method.-->

# Other

Q: Mobile devices (R1/R2):
<!-- (r1) experiment is done on high-end machine but paper is targeting for mobile device?
     ZH: We mentioned 'mobile' only once, in the introduction L160. Shall we try and report a number on mobile VR device?
     (r2) On what hardware is this method running in real-time
     ZH: CPU is Intel(R) Xeon(R) Gold 6230R CPU@2.10GHz (256GB RAM) and one NVIDIA GTX 3090 graphics card.
     (r2) distance towards low latency, and low-storage VR/AR platforms? -->

We target at future wearable VR/AR devices that are "akin to" (Ln160) mobile devices whose low storage and computational power demands dynamically fetching content from cloud. Our method enables fast fetching with only 0.5mb for a range of local camera 6DoF motions so that the system can keep running without any network-induced latency. Since it allows local translation, the method can fetch (for future positions) while rendering in parallel. 
That is, the research targets at the "constant" viewing experience without long pre-installation. It compares to the browser games vs. traditionally locally installed games.

<!--Further, on mobile devices, the supported resolution (PPD) is lower, demanding low inference computation.-->

<!--Qi: @Nianchen, the best if you could quickly try to run that on a mobile phone or having collaborators do that asap
We care about the experience without long installation, compariable to web-based gaming vs traditional gaming.
-->

Q: Static scene encoding (R1/R3/R4):
<!-- (r1) no user interaction, mobile devices can store
     (r3) scope should be clarified-->
We currently focus on encoding/decoding static scene in this project. Encoding dynamic and interactive scenes in neural scene representation is an orthogonal challenges that are getting attention to the neural rendering community: "D-NeRF: Neural Radiance Fields for Dynamic Scenes", arxiv 2020.
We thank R3 for the suggestion of clarifying the scope. We will tone down and state this limitation with static scenes and future attempts to extend the model with dynamic scenes while also incorporating temporal visual perception such as saccades (R4).

<!--## About encoding performance (r3.5, r4.3):
(r3) scope should be clarified
(r4) viewing is instant, but creation is not (encoding/training is slow)-->

This research assumes that the encoding of the scene (network training) is executed in a large-scale cloud service only once (which is practically available), so the computing overhead is ignored.

Q: User study (R1/R3/R4/R5):

<!-- (r1, r4) short, gaze fixed / static
     (r1) leave no time to experience ego-motion parallax effects
     (r3) narrow foveation methods selection-->

The stimuli's duration was designed according to the requirement (0.3-0.4s) of preventing refocusing and shifting (Ln811) as studied in:
"Dynamics of accommodation responses of the human eye" Campbell & Westheimer,  The J. physiology, 1960

Otherwise, the gaze changes will cause unfair comparisons, resulting in bias.

<!--We asked users to fixate at certain gaze positions so everyone's views were consistent across conditions and participants. (TBD) We set duration to 300ms so it is close to saccade experience(?)-->

<!-- 3/9
1) 300ms > per frame
2) even not enough, already proved ours > nerf
motion parallax is exactly our beneifts than panorama synthesis we will compare with panorama
it is fair for each view
-->

With eye-tracking in VR, foveated rendering is chose as GT because that has been shown to be perceptually identical to a full resolution rendering. Higher resolution on the peripheral range has minimal effects on perception. We have done an orthogonal pilot study verifying the identicality between F-GT and GT. We will include the details. 
<!-- we are compatible to any image-space foveation methods-->

Q: Source code (R3/R5):

Yes, we will make the entire source code publicly open, including models and data.


Q: Storage comparison with [Broxton2020] (R2)

First, we would like to point out a calculation mistake. The result of $(6/10)^3*10$ is not $0.06$ but $2.16$. Second, [Broxton2020] involves video compression while our is the raw size. Some practical compression methods can be applied to furthur reduce the size (to 0.3MB tested by Zip).

<!-- Q: Experiment about storage? (r2.3)
What baseline requires 100MB of storage claimed in the introduction? The original 3D asset? There must be a middle ground which improves storage somewhat, without having to evaluate a neural network for every output pixel. -->

Q: The comparison should allow NeRF an additional 96 channels (R4)

We are using the same parameters that were used in our setup. Nevertheless, the performance of such NeRF configuration is still very slow. 

<!--DNC: Same network parameters means same network capacity. Use large network capacity to compare our small network capacity is unfair.-->

Q: Why our method is better than baseline (i. e. NeRF)? (R4)

We use different coordinates (egocentric coordinates) while baseline uses 3D volume. We are for different tasks. 3D volume is not suitable for our inside-out view mode.

Q: Artifacts in video (R4)

We've checked that the double image artifacts (1m30) are artifacts caused by video codec. We will choose a proper codec for our demo video within minor revision cycles. <!-- black splotches, where they come from? (1m03) TO BE CHECKD-->

Q: Isn't the representation just alpha composited front to back? (R4)
Ray cast is different from alpha blending. it is an integration of density. We refer the reviewer to ["NeRF":Milden-hall et al.2020] for the detail.

## Should we reply to questions below?

Q: (Misunderstanding) need depth reconstruction for 360 photos application. (R4)

The neural encoding the scene information, including the depth. So we do not need extra depth information in run-time. Depth information can be inferred by ray casting using output density.

Q: Explanation to Fig. 7. (R4)

The bounding refers to the range of sampling in ray casting (the near and far plane), which affects the distribution of sampling distances. <!--we set upper bound to 10000 in (a) to approximate the infinite because sampling are done in disparity space ($1/distance$)-->


Q: Is NeRF here also running on a TensorRT implementation? (R4) 
No.
<!--ZH: explain the potential effects here.-->

Q: Composition of the user study (R5)
<!--"The entire experiment of each user consists of 36 trials, 6 trials per pair of ordered conditions." how did this come about? 3 pairs of conditions x 2 scenes? (r5.2)-->
<!--"Each condition from an individual scene consists of 3 different gaze positions." I did not understand what this meant. Once participants had moved their eyes 3 times, the experiment stopped? (r5.3)-->

6 trials (2 scenes x 3 gaze positions) x 6 pairs (permutation of 2 conditions). We predefined 3 gaze positions for each scene. Participants were asked to fixate each predefined gaze position for each trial and made the choice. 

Q:  What is meant by perceptual similarity? color similarity? (R5)
<!--"LPIPS uses deep neural networks to estimate perceptual similarities between the image provided and a reference image."-->

Perceptual similarity is not only color similarity. It indicates the similarity of human visual perception.
<!--ZH(TBD): IPIPS-->

Q: What is being treated as repeated measures? what is the independent and dependent variable? and what does "together" refer to? (R5)
<!--"We used one-way repeated measures ANOVAs to compare effects across three stimuli on each eccentricity range and together."-->

IV: three conditions (F-GT, OURS, NeRF). DV: LPIPS value at eccentricity=5, 10, 15, ... ,105.

<!--DNC Remark:
1. 把view-dependent作为我们的一个重要研究目标是否合适？因为我们在之前的submission中没有相关表述和评估，现在突然变成了一个重点在开篇被提及，会不会让人感觉很奇怪？
2. PDD是一个质量相关的问题，虽然R2主要针对的不是我们的质量问题，但如果这个“WHY”我们不予回答，会不会让R2更加认为我们的方法不够好？
3. About comparison with foveation rendering method 看起来也是一个比较共性的问题（r1.5, r2.1, r3.4）-->

<!--
1. fixed
2. Why means since your res is already that low, WHY do you bother doing a nerf than just prebake. I think we answered the nessisity of using nerf than panorama/MPI
Re(DNC): I mean this: >>>
   In Technical Correctness and Reproducibility of R2:
   However, I found the statements on resolution to be inconsistent throughout the paper. In Section 3.2.1 the maximum angular resolution of the system is stated to be 6.4 PPD, however, the experiments compare with high resolution 13 PPD images (1440x1600 with a 110 FOV). How come no one was able to pick up on this drastic change in resolution during the user study. Were the ground truth images blurred to a maximum of 6.4PPD?
   <<< Anyway, it's a problem related to 3 so we can discuss them together later.
3. Yeah, I dont have a good clue of that yet. Do you have some good answers/excuses? Maybe ask Zhenyi too.
  Re(DNC): May be we can discuss later
-->