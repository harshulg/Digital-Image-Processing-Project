# Digital-Image-Processing-Project
In this project, we implemented a simple image prior—dark channel prior to remove  haze from a input image. The dark channel prior is a kind of statistical model of  outdoor haze-free images. It is based on a key observation—most local patches in  outdoor haze-free images contain some pixels whose intensity is very low in at least  one color channel. Using this prior with the haze imaging model, we can directly  estimate the thickness of the haze and recover a high-quality haze-free image.  Results on a variety of hazy images demonstrate the power of the proposed prior.  Moreover, a high-quality depth map can also be obtained as a byproduct of haze  removal.
# INTRODUCTION
I many times came across the outdoor images that generally degraded by presence
of several things in the atmosphere. Haze, fog, and smoke are such phenomena due 
to atmospheric absorption and scattering. The irradiance received by the camera 
from the scene point is attenuated along the line of sight. Furthermore, the 
incoming light is blended with the airlight —ambient light reflected into the line of 
sight by atmospheric particles.As a result images lose contrast and color fidelity. 
Since the amount of scattering depends on the distance of the scene points from 
the camera, the degradation is spatially variant. Haze removal(or dehazing) is highly 
desirable technique in consumer/computational photography and Image processing
applications. First, removing haze can significantly increase the visibility of the scene 
and correct it’s color shift caused by the airlight. The haze-free image is more 
visually pleasing. Second, most computer vision algorithms, from low-level image 
analysis to high-level object recognition, usually assume that the input image is the 
scene radiance. The performance of many vision algorithms (e.g., feature detection, 
filtering, and photometric analysis) will inevitably suffer from the biased and lowcontrast scene radiance. Last, haze removal can provide depth information and 
benefit many vision algorithms and advanced image editing. Haze or fog can be a 
useful depth clue for scene understanding. We can put A bad hazy image to good 
use.
# Background
In this project, model we used to describe the formation of a haze image is as 
follows 
I(x) = J(x)t(x) + A(1 − t(x))
where I is the observed intensity, J is the scene radiance, A is the global atmospheric 
light, and t is the medium transmission describing the portion of the light that is not 
scattered and reaches the camera. The goal of haze removal is to recover J, A, and t 
from I
The first term J(x)t(x) on the right hand side of Equation is called direct attenuation , 
and the second term A(1 − t(x)) is called airlight .Direct attenuation describes the 
scene radiance and its decay in the medium, while airlight results from previously 
scattered light and leads to the shift of the scene color.
# Dark Channel Prior
◦ The dark channel prior is based on the following observation on haze-free 
outdoor images: in most of the non-sky patches, at least one color channel 
has very low intensity at some pixels. In other words, the minimum intensity 
in such a patch should has a very low value. Formally, for an image J, we 
define
◦
◦ where Jc is a color channel of J and Ω(x) is a local patch centered at x. Our 
observation says that except for the sky region, the intensity of J dark is low 
and tends to be zero, if J is a haze-free outdoor image. We call Jdark the dark 
channel of J, and we call the above statistical observation or knowledge the 
dark channel prior

# Estimating the Atmospheric Light
◦ we first assume that the atmospheric light A is given. We will present an 
automatic method to estimate the atmospheric light
◦ In most of the previous single image methods, the atmospheric light A is 
estimated from the most haze-opaque pixel.. But in some cases, the brightest 
pixel could be on a white car or a white building.
◦ To avoid that problem We can use the dark channel to improve the 
atmospheric light estimation. We first pick the top 0.1% brightest pixels in the 
dark channel. Among these pixels, the pixels with highest intensity in the 
input image I is selected as the atmospheric light
Estimating the Transmission
◦ We assume that the transmission in a local patch Ω(x) is constant. We denote 
the patch’s transmission as t ˜(x). Taking the min operation in the local patch 
on the haze imaging Equation, we have:
◦
◦ The value of ω is application-based. We assumed it to be 0.95.
◦ In practice, even in clear days the atmosphere is not absolutely free of any 
particle. So, the haze still exists when we look at distant objects. If we remove 
the haze thoroughly, the image may seem unnatural and the feeling of depth 
may be lost. So we can optionally keep a very small amount of haze for the 
distant objects by introducing a constant parameter ω

# Soft Matting
◦ we apply a soft matting algorithm to refine the transmission.
◦ Denote the refined transmission map by t(x)
◦ we minimize the following cost function:
◦
◦ L is the Matting Laplacian matrix
Recovering the Scene Radiance
◦ The final scene radiance J(x) is recovered by:
◦
◦ A typical value of t0 is 0.1.Since the scene radiance is usually not as bright as 
the atmospheric light, the image after haze removal looks dim. So, we 
increase the exposure of J(x) for display
Result
Input:-
 
6
#Output:-
#References
K. He, J. Sun and X. Tang, "Single Image Haze Removal Using Dark Channel Prior," in 
IEEE Transactions on Pattern Analysis and Machine Intelligence, vol. 33, no. 12, pp. 
2341-2353, Dec. 2011, doi: 10.1109/TPAMI.2010.168.
