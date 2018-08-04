![feature image][1]

Source Code: [Github][2]

Remember "[the dress][3]" that went viral in 2015? Viewers disagreed over whether the dress was black and blue or white and gold, and even the same person can see totally different colors when they see the photo for the second time. Some articles published at that time explained the biological aspect of the matter - how we can be deceived by our brains. Today, we will see how colors really distribute in the color spectrum by using Wolfram Language.

First, we will consider the color space being used. Below is the XYZ Color Space created by the International Commission on Illumination (CIE) in 1931, namely the [CIE 1931 color space][4]:

    ChromaticityPlot["sRGB", WhitePoint -> None]

![XYZ Color Space][5]

**Image 1: Chromaticity Plot of the CIE 1931 color space**

As we can see, the outer curved boundary is the spectral (or monochromatic) locus (for details, please see [this article][6]), while the triangle represents the [standard Red Green Blue color space][7] (i.e. sRGB) created by HP and Microsoft in 1996. The vertexes of the triangle are the "pure" Red, Green and Blue colors that we normally see on computers.

Let's see how "blue" and "gold" the dress are by singling out these two colors, i.e. "Royal blue" and "Gold". According to Wikipedia, their coordinates on the sRGB color space are [(65, 105, 225)][8] and [(255, 215, 0)][9] respectively. We would transform these two coordinates back to the XYZ space as below:-

$ \begin{bmatrix} X \\ Y \\ Z \end{bmatrix} = \begin{bmatrix} 0.4124 & 0.3576 & 0.1805 \\ 0.2126 & 0.7152 & 0.0722 \\ 0.0193 & 0.1192 & 0.9505 \end{bmatrix} \begin{bmatrix} R \\ G \\ B \end{bmatrix} $

Now, we have the Chromaticity Plot as below:-

![color with names][10]

**Image 2: Chromaticity Plot for the Royal Blue and Gold color**

We now import the image of the dress as `imgB` and see how the colors distribute in the color space:-

    Show[ChromaticityPlot[#], plotCircles] &@imgB

![color with names][11]

**Image 3: Distribution of the color of "the dress" in the color space**

We can see that the colors of this disputed image are distributed along Royal blue and Gold!

We now import two more images, `imgA` and `imgC`, which are the color corrected versions of the original image. Most people would see Gold in `imgA` (corrected for blue light) and Blue in `imgC` (corrected for yellow light).

![Three dresses][12]

**Image 4: Color corrected versions of the original image.**

We would like to know how the colors of these three photos distribute in the color space. But we would like to be more accurate about the coloring distribution, and we do not want the background to mess with the distribution. We can remove the background by using an [alpha][13]:-

    background = White;
    img10alpha = Binarize[ColorNegate@alpha, 0.99];
    imgABCNoBackground = ImageMultiply[ImageAdd[#, img10alpha], ImageAdd[ColorNegate@img10alpha, background]] & /@ imgABC

![Three dresses and three plots - No Background][14]

**Image 5: Color distribution of the different corrected versions of "the dress" (with the background removed) in the color space**

As you can see, for `imgA`, colors shift to the Yellow/Gold region. While for `imgC`, they shift to the Blue region.

References
-----------------

- [A Beginner’s Guide to (CIE) Colorimetry][15]
- [sRGB Color Space][7]
- [CIE 1931 Color Space][4]


  [1]: http://community.wolfram.com//c/portal/getImageAttachment?filename=1.FeatureImage.png&userId=1353389
  [2]: https://github.com/lanstonchu/CIE1931ColorSpace
  [3]: https://en.wikipedia.org/wiki/The_dress
  [4]: https://en.wikipedia.org/wiki/CIE_1931_color_space
  [5]: http://community.wolfram.com//c/portal/getImageAttachment?filename=2.XYZSpace.jpg&userId=1353389
  [6]: https://medium.com/hipster-color-science/a-beginners-guide-to-colorimetry-401f1830b65a
  [7]: https://en.wikipedia.org/wiki/SRGB
  [8]: https://en.wikipedia.org/wiki/Royal_blue
  [9]: https://en.wikipedia.org/wiki/Gold_%28color%29
  [10]: http://community.wolfram.com//c/portal/getImageAttachment?filename=3.colorwithnames.jpg&userId=1353389
  [11]: http://community.wolfram.com//c/portal/getImageAttachment?filename=1.FeatureImage.png&userId=1353389
  [12]: http://community.wolfram.com//c/portal/getImageAttachment?filename=4.Threedresses.png&userId=1353389
  [13]: https://en.wikipedia.org/wiki/Alpha_compositing
  [14]: http://community.wolfram.com//c/portal/getImageAttachment?filename=5.Threedressesandthreeplots-NoBackground.png&userId=1353389
  [15]: https://medium.com/hipster-color-science/a-beginners-guide-to-colorimetry-401f1830b65a
