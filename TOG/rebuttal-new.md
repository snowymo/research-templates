We thank the reviewers' valuable comments. Allow us to address the main points, followed by detailed questions.

We would like to emphasize that the main contribution of this work is devising a human-vision-matched (resolution and stereo acuity) 6DoF neural representation that can be inferred in real-time for VR near-eye displays. The method is applicable for not only virtual environments but also physical captures/spaces, compatible with view-dependent effects for future low-storage, wearable and cloud-based VR/AR where only the pre-trained scene models will need to be dynamically transmitted. The research addresses the high-latency (9s per frame, Table. 1) problem in current neural representation, and the limited expressiveness (such as correct lighting/illumination effects) in the recent image (panorama or multi-layer image)-based representation/re-projection methods.

# The neural representation method

Q: Comparison with traditional mesh representation (R2/R4):

First, there is a misunderstanding about the applicable case of the work. Our (and NeRF's) goal is not about encoding mesh-based virtual scenes. The neural network is a representation of radiance field. That is, besides virtual scenes, physical scenes just follow exactly same computation procedure. The only data we need to train such a model are several RGB images with corresponding intrinsic and extrinsic parameters.
We apologize for not including such examples and will include new examples with both physical scenes and dynamic lighting effects to visually demonstrate the necessity.

Further, the network is not related to the complexity of scenes. The size of a mesh scene is related to its geometric and textural fineness, which may be extremely large (e.g. hundreds or even thousands MB) with full visibility (e.g., terrain), but the size of the network is a constant (0.5MB in our experiments). In the cloud-based application scenario that all data are acquired from cloud services at run-time, large data size not only leads to high storage requirement, but also increases the transmission and loading latency.

Q: Comparison with MSI methods (R2):

First, our neural synthesis method naturally support correct dis-occlusions and motion parallax as the representation treats each view uniformly inside the motion range. Meanwhile, for physical scenes, MSI methods (e.g. panorama-based pre-baking) require additional image-based inference of occlusions and depths. Ln267 mentions that these methods "typically suffers from trade-offs among performance, achievable resolution, and dis-occlusion effects due to lack of knowledge of the 3D space". Directly recording a precise multi-layer panorama requires sophisticated devices.

Second, similar to NeRF, our method does support view-dependent effect that is lacking with image-based panorama pre-baking solutions. In the current demo, we didn't include global illumination for faster data preparation. To clearly demonstrate the difference, we will include a visual comparison with panorama+disocclusion method as well as examples with global illumination.

Q: The motion range and parallax support (R1/R2/R4)

Our gaze range covers the entire display-supported FoV ($100^\circ$). The rotational range is only determined by the computing resources and duration for training. A larger rotation means a larger training set (longer training time).  In the "gallery" scene, we trained for up to $240^\circ$ rotational range. We are confident to include a $360^\circ$ rotational result within a minor revision cycle.
For head volume, in our experiments, we tested $30cm^3$. Ln1068-1070 and Fig.11 depict the balance: although computationally the innermost sphere (that defines the supported translation range) could any large, significantly enlarge it would cause visual quality decreasing.
Developing a multi scale network that synthesizes the image from global to local level-of-details would be an interesting direction for future work, extending the applicability of our method to large-scale scenes with strong occlusions. We support motion parallax and occlusion within the range determined by the innermost sphere. Unlike the pure rotation method, we do not need to train a network for every viewpoint.

We support correct occlusion and motion parallax at any view position in the range of motion. These visual effects are demonstrated, although not significantly visible, in the video (1m20s ~ 1m25s). We will include larger translation to demonstrated more significant occlusion and motion parallax within a minor revision cycle.

<!--DNC: Do we need further explanation about NeRF to R4?��-->

> Q: (Misunderstanding) need depth reconstruction for 360 photos application. (R4)
>
> The neural encoding the scene information, including the depth. So we do not need extra depth information in run-time. Depth information can be inferred by ray casting using output density.
>
> Q: Isn't the representation just alpha composited front to back? (R4)
>
> Ray cast is different from alpha blending. it is an integration of density. We refer the reviewer to ["NeRF":Milden-hall et al.2020] for the detail.

# Validation

Q: Explanation about user study (R1/R3/R4/R5):

As Ln788 mentioned, two scenes (with 3 predefined gaze positions for each scene) are shown in a test. Participants were asked to fixate the predefined gaze positions. Three methods (F-GT/Our/NeRF) are involved, leading to 6 combinations of method pairs for each trial. So there are $2\times3\times6=36$ trials in total.

The stimuli's duration was designed according to the requirement (0.3-0.4s) of preventing refocusing and shifting (Ln811) as studied in:
"Dynamics of accommodation responses of the human eye" Campbell & Westheimer,  The J. physiology, 1960. Otherwise, the gaze changes will cause unfair comparisons, resulting in bias.

<!-- we are compatible to any image-space foveation methods-->

<!-- DNC: @ZH, please help me merge below questions into one. seems they all about section 5.2-->

Q:  What is meant by perceptual similarity? color similarity? (R5)
<!--"LPIPS uses deep neural networks to estimate perceptual similarities between the image provided and a reference image."-->

Perceptual similarity is not only color similarity. It indicates the similarity of human visual perception.
<!--ZH(TBD): IPIPS-->

Q: What is being treated as repeated measures? what is the independent and dependent variable? and what does "together" refer to? (R5)
<!--"We used one-way repeated measures ANOVAs to compare effects across three stimuli on each eccentricity range and together."-->

IV: three conditions (F-GT, OURS, NeRF). DV: LPIPS value at eccentricity=5, 10, 15, ... ,105.

# Other

Q: Mobile devices (R1/R2):

We target at future wearable VR/AR devices that are "akin to" (Ln160) mobile devices whose low storage and computational power demands dynamically fetching content from cloud. Our method enables fast fetching with only 0.5mb for a range of local camera 6DoF motions so that the system can keep running without any network-induced latency. Since it allows local translation, the method can fetch (for future positions) while rendering in parallel. 
That is, the research targets at the "constant" viewing experience without long pre-installation. It compares to the browser games vs. traditionally locally installed games.

<!--Further, on mobile devices, the supported resolution (PPD) is lower, demanding low inference computation.-->
<!--Qi: @Nianchen, the best if you could quickly try to run that on a mobile phone or having collaborators do that-->

Q: Static scene encoding (R1/R3/R4):
We currently focus on encoding/decoding static scene in this project. Encoding dynamic and interactive scenes in neural scene representation is an orthogonal challenges that are getting attention to the neural rendering community: "D-NeRF: Neural Radiance Fields for Dynamic Scenes", arxiv 2020.
We thank R3 for the suggestion of clarifying the scope. We will tone down and state this limitation with static scenes and future attempts to extend the model with dynamic scenes while also incorporating temporal visual perception such as saccades (R4). This research assumes that the encoding of the scene (network training) is executed in a large-scale cloud service only once (which is practically available), so the encoding overhead is ignored.

Q: Source code (R3/R5):

Yes, we will make the entire source code publicly open, including models and data.


Q: Storage comparison with [Broxton2020] (R2):

First, we would like to point out a calculation mistake. The result of $(6/10)^3*10$ is not $0.06$ but $2.16\mathrm{MB}$. Second, [Broxton2020] involves a video compression while the size we report is for the original data. Some practical compression methods can be applied to further reduce the size (0.3MB tested by Zip).

Q: Why our method is better than baseline (i. e. NeRF)? (R4)

We are for different tasks. We use egocentric coordinates which is more suitable for out inside-out view mode (while baseline uses 3D volume). We cannot agree with the opinion that "a fairer comparison would allow NeRF an additional $96$ channels to represent the whole scene". We think that the comparison with NeRF using the same parameters as our method is fair because both networks have same capacity.  Nevertheless, the performance of such NeRF configuration is still very slow. <!--DNC: Expect a more appropriate expression -->

Q: Artifacts in video (R4):

We've checked that the double image artifacts (1m30) are artifacts caused by video codec. We will choose a proper codec for our demo video within minor revision cycles. <!-- black splotches, where they come from? (1m03) TO BE CHECKED-->

## Should we reply to questions below?

Q: Experiment about storage? (R2)

What baseline requires 100MB of storage claimed in the introduction? The original 3D asset? There must be a middle ground which improves storage somewhat, without having to evaluate a neural network for every output pixel.

Q: Explanation to Fig. 7. (R4)

The bounding refers to the range of sampling in ray casting (the near and far plane), which affects the distribution of sampling distances. <!--we set upper bound to 10000 in (a) to approximate the infinite because sampling are done in disparity space ($1/distance$)-->

Q: Is NeRF here also running on a TensorRT implementation? (R4) 
No.
<!--ZH: explain the potential effects here.-->
