# Threshold

Julien Benetti*, Alexis Hubert et Rémy Viannais

https://github.com/rmy17/bioinf-struct/tree/master/

## Introduction

Thresholding is the simplest method of image segmentation. Bilevel thresholding divides a gray-scale image into two classes of pixels, typically objects and background. To do so, a threshold value is selected arbitrarily, each pixel with an intensity smaller than the threshold value is replaced by a black pixel, each pixel larger by a white pixel - or vice versa.

Thresholding can be trilevel with two thresholds : the pixels outside are replaced by black or white pixels, those inside by the other kind.

Thresholding can be multilevel : many thresholds placed either equidistantly or according to clusters of pixels intensity are used - often 3 or 7 to have 4 or 8 levels of discrimination. Those levels are then turned in different shades of gray - most of the time the barycenter of the level/cluster.

Finally with multi-band thresholding, color images can be processed. One approach is to designate a separate threshold for each of the RGB components of the image and then combine them with an AND operation. This reflects the way the camera works and how the data is stored in the computer, but it does not correspond to the way that people recognize color. Therefore, the HSL and HSV color models are more often used.

In addition to all this, thresholding can be either global - like described above - or local. Global thresholding categorizes a pixel as part of an object or the background based on a single threshold value derived from the intensity statistics of the entire image. Local thresholding categorizes a pixel based on the intensity statistics of its neighboring pixels.  It is used to isolate objects of interest from the background in images that exhibit nonuniform lighting changes. Nonuniform lighting changes, such as those resulting from a strong illumination gradient or shadows, often make global thresholding ineffective.

Thresholding can be automated. If the object is clearly distinguishable from the background, the gray-level histogram will be bimodal and the threshold for segmentation can be chosen at the bottom of the valley. However, gray-level histograms are not always bimodal.
Many research studies have been devoted to determine the appropriate threshold value and therefore many methods exist. Those methods can be categorized based on the information the algorithm manipulates : Histogram-shape where the peaks, valleys and curvatures of the histogram are analyzed, Entropy - i.e. the amount of information - of the foreground and background regions or the cross-entropy between the original and binarized image, Object-attribute that measures the similarity of attribute between the gray-level and the binarized images, such as fuzzy shape similarity or edge coincidence and other categories.

How to know if a threshold is correct ? In short, it can't. It will always be, to some extent, in the eye of the user and will also be impacted by empirically collected knowledge. The basic problem of deciding if a threshold - or in general an extraction method - is "good" needs a "ground truth". But such a ground truth is not naturally existing and is always created in one or the other way by a human.


Amongst the auto-thresholding methods described further only the ones processing gray-scale images with a global bilevel thresholding will be implemented.

## Material and Methods

### ImageJ

ImageJ is a Java-based image processing program developed at the National Institutes of Health. ImageJ was designed with an open architecture that provides extensibility via Java plugins and recordable macros. User-written plugins make it possible to solve many image processing and analysis problems. ImageJ's plugin architecture and built-in development environment has made it a popular platform for teaching image processing.

It can display, edit, analyze, process, save, and print 8-bit color and grayscale, 16-bit integer, and 32-bit floating point images. It can read many image file formats, including TIFF, PNG, GIF, JPEG, BMP, DICOM, and FITS, as well as raw formats. ImageJ supports image stacks and it is multithreaded, so time-consuming operations can be performed in parallel on multi-CPU hardware.

Amongst ImageJ's methods, there is the thresholding introduced above. Some of the methods will be described below, the list is not exhaustive.

### Auto-thresholding

Global gray-scale bilevel Auto-Thresholding :

Huang[^LEU2002]: Thresholding method based on minimizing the measures of fuzziness of an input image and the membership of each pixel. The index of fuzziness correspond to the entropy and can be calculated using Shannon's function or Yager's measure. The membership of a pixel is defined by the absolute difference between its gray level and the average gray level of its surrounding. The threshold value is then iteratively changed from a minimum to a maximum and the optimal threshold is then selected according to the previous data.

Intermodes[^PRE1966]: The intermodes method assumes a bimodal histogram. The histogram is iteratively smoothed using a running average of size 3, until there are only two local maxima: j and k. The threshold t is then computed as (j+k)/2. This method is unsuitable for images that have a histogram with extremely unequal peaks.

Isodata[^RID1978]: Based on the isodata algorithm of Ridler. After an initial threshold, isodata method divides the image into background and object. Then the average of pixel values below the threshold and above are computed, the average of those two are computed. The threshold is then incremented and the process is repeated until the threshold is larger than the composite average.

Li [^LI1993],[^LI1998],[^SEZ2004]: This method is called the minimum cross entropy method, it selects the threshold which minimizes the cross entropy of the image and its segmented version.

MaxEntropy[^KAP1985]: The entropy of grayscale image is statistical measure of randomness that can be used to characterize the texture of the input image. It is used to describe the business of an image, i.e. the amount of information which must be coded for by a compression algorithm. This method use the derivation of the probability distribution of gray-levels to define the entropies and then find the maximum information between the object and the background - the entropy maximum, the threshold value.

Mean[^GLA1993]: The threshold is set such that it is the integer part of the mean of all pixel values. It does not take into account histogram shape, so obviously the results are suboptimal.

MinError[^KIT1986]: A computationally efficient solution to the problem of minimum error thresholding is derived under the assumption of object and pixel gray level values being normally distributed. The principal idea behind the method is to optimise the average pixel classification error rate directly, using either an exhaustive search or an iterative algorithm. It removes the equal variance assumption and addresses a minimum error Gaussian density-fitting problem.

Minimum[^PRE1966] : Like the Intermodes method, this assumes a bimodal histogram. The histogram is iteratively smoothed using the three-point filter, until the histogram has only two local maxima. The threshold t is such that yt-1 > yt <= yt+1. This method is unsuitable for images taht have a histogram with extremely unequal peaks or a broad and flat valley.

Moments[^TSA1985] : This approach may be regarded as a moment-preserving image transformation which recovers an ideal image from a blurred version. The approach select multiple thresholds without iteration or search. The moments method can be use for bilevel and multilevel thresholding but we are going to work only on the bilevel.
For bilevel threshold, this method consist in select a threshold value such that if all below-threshold gray values in the image are replaced by x and all above-threshold gray values replaced by y, where x<y, then the first three moments of the image are preserved in the resulting bilevel image. Image so obtained may be regarded as an ideal unblurred version of the initial image.

Otsu[^OTS1979] : Otsu method is a nonparametric and unsupervised method of automatic threshold selection for picture segmentation. The algorithm assumes that the image contains two classes of pixels following bi-modal histogram(foreground and background) and search for the threshold that minimizes the intra-class variance (weighted sum of variances of the two classes).

Percentile[^DOY1962] : The principle of this method is to choose the threshold value such that 50\% of pixels lie in each binary categories. Obviously the results are suboptimal.

RenyiEntropy[^KAP1985] : It is the same method as MaxEntropy but instead of using Shannon's function to calculate the entropy, it uses Rényi entropy that generalizes the Hartley entropy, the Shannon entropy, the collision entropy and the min entropy.

Shangbhag[^SHA1994] : This algorithm is entropic based. It views the image as a compositum of two fuzzy sets corresponding to the two classes with membership coefficient associated with each gray level a function of its frequency of occurrence as well as its distance from the intermediate threshold selected.

`threshold = abs(entropyBackground - entropyObject)`

Triangle[^ZAC1977] : The histogram is enclosed in a right triangle between the maximum peak and the end of the histogram bins. The objective is to find the histogram bin with the greatest distance between its top and the hypotenuse of the triangle, in a line perpendicular to the hypotenuse.

Yen[^YEN1995],[^SEZ2004] : A new criterion for multilevel thresholding is proposed. The criterion is based on the consideration of two factors. The first one is the discrepancy between the thresholded and original images and the second one is the number of bits required to represent the thresholded image. Based on a new maximum correlation criterion for bilevel thresholding, the discrepancy is defined and then a cost function that takes both factors into account is proposed for multilevel thresholding. By minimizing the cost function, the classification number that the gray-levels should be classified and the threshold values can be determined automatically.




### Local auto-thresholding

In this class of algorithm, a threshold is calculated at each pixel, which depends of on some local statistics like range, or surface-fitting parameters of the pixel neighborhood.

Bernsen's thresholding method uses a user-provided contrast threshold. Then if the local contrast is equal or above to the contrast threshold, the treshold is set at the mean of the minimum and maximum gray values in the local windows. However, if the local contrast is below the contrast threshold, the neighbourhood is considered to consist only of one class and the pixel is set to object or background depending on the value of the midgrey.

	if ( local contrast < contrast threshold )
	pixel = ( mid gray >= 128 ) ? object : background
	else
	pixel = ( pixel >= mid gray ) ? object : background


Contrast method : It is based on a simple contrast toggle. This procedure does not have user-provided parameters other than the kernel radius. This it sets the pixel value to either white or black depending on whether its current value is closest to the local Max or Min respectively.

`pixel = ( pixel > ((min+max)/2) ? object : background`

Mean, Median, MidGrey : To find the local threshold the intensity values of the local neighborhood of each pixel can be statistically examined. The statistics which is more appropriate, depends largely on the input image. So a choice can be made between the mean, the median and the "midgrey" (average of the maximum grey intensity and the minimum grey intensity) in function of the input image. The size of the neighborhood has to be large enough to cover sufficient foreground and background pixels, otherwise the chosen threshold is not good enough. However, choosing a large region can violate the the assumption of approximately uniform illumination.

`pixel = ( pixel > mean - c ) ? object : background`

`pixel = ( pixel > median - c ) ? object : background`

`pixel = ( pixel > ( ( max + min ) / 2 ) - c ) ? object : background`

where c is a constant.

Otsu : It is a local version of Otsu's global threshold clustering. In the same way, the algorithm searches for the threshold that minimizes the intra-class variance, defined as a weighted sum of variances of the two classes.
The local set is a circular ROI and the central pixel is tested against the Otsu threshold found for that region.

Niblack[^NIB1986],[^SEZ2004]: It is an improved local mean threshold that adds the standard deviation and two constants. The first one mutiplies the standard deviation value -- the default value is 0.2 for bright objects and -0.2 for dark objects. The second substracts the standard deviation value.

`pixel = ( pixel >  mean + k * standard deviation - c) ? object : background`

Sauvola[^SAU2000]: It is an improvement on the Niblack method, especially for stained and badly illuminated documents. It adapts the contribution of the standard deviation in an adaptive manner.

`pixel = ( pixel > mean * ( 1 + k * ( standard deviation / r - 1 ) ) ) ? object : background`

Phansalkar[^PHA2011]: This is a modification of Sauvola's thresholding method to deal with low contrast images.
In this method, the threshold t is computed as:
`t = mean * (1+ p * exp(-q * mean) + k * ((standard deviation / r ) - 1))`

mean are the local mean and k and r are the paramters 1 and 2 respectively p and q are fixed.
Phansalkar recommends k = 0.25, r = 0.5, p = 2 and q = 10.

### Color thresholding

Color thresholding methods as known as multi-band thresholding use the same thresholding methods as global thresholding. Indeed, it just separates each channel of the color image, applies a global thresholding methods and then merge all of the three channels with an AND operation into one final image.

### Benchmark


The benchmark in computing is an act of running a computer program, in order to assess the relative performance of it, normally by running a number of standard tests. It is usually associated with assessing performance characteristics of computer performance.
Only the time were benchmarked here, since the impacts on the memory and the CPU were fluky and insignificant compared to the loading of the image.

## Results

We used a set of three different images to test all the methods of automatic thresholding. A stump seen from above, a cross section of a microscopic image and a portrait - Lena. All of them were time-benchmarked with all of the methods 10.000 times. Since there is a large gap between the fastest methods and the longest methods, the measurements have been logarithmized.


<figure>
    <img src="images/coupe_tronc.png" alt="Image" />
    <center><figcaption>Figure 1 : Results of all thresholding methods on the stump image.
	From the up-left corner to the down-right corner : Default, Huang, Intermodes, IsoData, Li, MaxEntropy, Mean, MinError(I), Minimum, Moments, Otsu, Percentile, RenyiEntropy, Shanbhag, Triangle, Yen. </figcaption></center>
</figure>



<figure>
    <img src="images/coupe_longitudinale.png" alt="Image" />
    <center><figcaption>Figure 2 : Results of all thresholding methods on the cross section of a cell.</figcaption></center>
</figure>

<figure>
    <img src="images/lena.png" alt="Image" />
    <center><figcaption>Figure 3 : Results of all thresholding methods on the portrait of Lena.</figcaption></center>
</figure>


Table.1. Results of the benchmark of all thresholding methods on the three different images in microseconds.

.    | Def  | Hua. | Int. | Iso. | Li   | Max. | Mea. | M.E. | Min. | Mom. | Ots. | Per. | R.E. | Sha. | Tri. | Yen
---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----
Stump | 16.7 | 39.46 | 20.33 | 6.03 | 1.06 | 481.58 | 0.29 | 26.55 | 21.36 | 1.36 | 1.37 | 10.43 | 765.4 | 573.23 | 0.49 | 8.06
Cross Section | 49.24 |90.96 | 49.45 | 17.23 | 1.49 | 1379.57 | 0.41 | 10.84 | 51.81 | 2.32 | 2.4 | 28.94 | 2131.83 | 1464.14 | 0.81 | 13.11
Lena | 26.98 | 61.39 | 109.73 | 9.61 | 1.34 | 906.16 | 0.34 | 53.89 | 114.12 | 1.42 | 1.44 | 18.61 | 1398.1 | 1030.09 | 0.73 | 10.45


<figure>
    <img src="images/chart(2).png" alt="Image" />
    <center><figcaption>Figure 4 :  Logarithmized chart results of the benchmark of all thresholding methods on the three different images.</figcaption></center>
</figure>


## Discussion

The results of the benchmark show that the different threshold methods have important differences in execution time. These differences are correlated with the complexity of the methods.  However, this simple criterion of speed is not sufficient to be able to classify these methods performances. Indeed, even if a "good" threshold is quite subjective, some of the results are obviously inadequate. We can add that time performances are not necessarily negatively correlated with good threshold performances. Threshold quality will greatly depends on the image type, some methods will perform better than others on certain images and vice-versa - as seen on the images in the results section.

### Improvements

Ideally, a machine learning tool could potentially improve the automation of thresholding. It would be fed with thousands of thresholded images either by hand or by a human decision involving one of the methods described above so it could learn a seemingly empirical knowledge quite relevant and then it will potentially be able to choose the best way to threshold any image given.


## Conclusion

There are many methods of automatic thresholding based on different basic principles (entropy, histogram, clustering). It is difficult to know which one is the most efficient. There are also local threshold methods that are more complex and quite harder to implement. Thresholds for color images also exist using the global method thresholding on the various color channels. There are even hybrid methods mixing the global threshold and local threshold.

## References

[^HUA1995]: Huang L-K, Wang M-JJ. Image thresholding by minimizing the measure of fuzziness. Pattern Recognition. 1995 Jan ;28(1):41-51.

[^PRE1966]: Prewitt JMS, Mendelsohn ML. The analysis of cell images.Annals of the New York Academy of Sciences. 1966 Jan ;128:1035-1053.

[^RID1978]: Ridler TW, Calvard S.Picture thresholding using an iterative selection method. IEEE Transactions on Systems, Man and Cybernetics. 1978 Aug ;8:630-632.

[^LI1993]: Li CH, Lee CK. Minimum Cross Entropy Thresholding.Pattern Recognition. 1993 Apr;26(4):617-625.

[^LI1998]: Li CH, Tam PKS. An Iterative Algorithm for Minimum Cross Entropy Thresholding. Pattern Recognition Letters. 1998 Mar;18(8):771-776.

[^SEZ2004]: Sezgin M, Sankur B. Survey over Image Thresholding Techniques and Quantitative Performance Evaluation. Journal of Electronic Imaging. 2004 Jan ;13(1):146-165.

[^KAP1985]: Kapur JN, Sahoo PK, Wong ACK.A New Method for Gray-Level Picture Thresholding Using the Entropy of the Histogram.Graphical Models and Image Processing. 1985 Mar;29(3):273-285.

[^GLA1993]: Glasbey CA.An analysis of histogram-based thresholding algorithms. CVGIP: Graphical Models and Image Processing. 1993 Nov;55:532-537.

[^KIT1986]: Kittler J, Illingworth J.Minimum error thresholding. Pattern Recognition. 1986 Jul;19:41-47.

[^TSA1985]: Tsai W.Moment-preserving thresholding: a new approach. Computer Vision, Graphics, and Image Processing. 1985 Aug ;29:377-393.

[^OTS1979]: Otsu N.A threshold selection method from gray-level histograms. IEEE Transactions on systems, man and cybernetics. 1979 Jan;9:62-66.
[^DOY1962]: Doyle W.Operation useful for similarity-invariant pattern recognition. Journal of the Association for Computing Machinery. 1962 Apr;9:259-267.

[^SHA1994]: Shanbhag AG.Utilization of information measure as a means of image thresholding. CVGIP: Graphical Models and Image Processing. 1994 Sep ;56(5):414-419.

[^ZAC1977]: Zack GW, Rogers WE, Latt SA. Automatic measurement of sister chromatid exchange frequency. Journal of Histochemistry & Cytochemistry. 1977 Jul;25(7):741–53.

[^YEN1995]: Yen JC, Chang FJ, Chang S.A New Criterion for Automatic Multilevel Thresholding. IEEE Transactions on Image Processing. 1995 Mar;4(3):370-378.

[^NIB1986]: Niblack W. An introduction to Digital Image Processing. New Jersey: Prentice-Hall; 1986.

[^SAU2000]: Sauvola J , Pietaksinen M. Adaptive Document Image Binarization. Pattern Recognition. 2000 Fev;33(2):225-236.

[^PHA2011]: Phansalskar N, More S ,Sabale A. Adaptive local thresholding for detection of nuclei in diversity stained cytology images. International Conference on Communications and Signal Processing. 2011 Mar;218-220.
