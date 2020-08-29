---
title: "Colab Rose - Python tutorial"
date: 2020-08-29T20:15:07+07:00
draft: false
tags: [Python, Colab]
featured_image: "https://i1.ytimg.com/vi/jPY1WmdcQa8/maxresdefault.jpg"
description: "Create hundreds of different images and videos with the 'Maurer rose'"
---
Tutorial code:
[click here to open](https://colab.research.google.com/github/kodindonesia/COLAB_Canvas-Video_Tutorials/blob/master/02_Tutorial_Rose.ipynb   "open this tutorial code, login in Google, and click 'save to drive'")

Manual to draw and to make videos code:
[click here to open](https://colab.research.google.com/github/kodindonesia/COLAB_Canvas-Video_Tutorials/blob/master/00_Manual_Canvas_cv_and_Video_cv.ipynb   "open this Colab notebook, and learn how to use the Kodindonesia helper")

* * *
{{< youtube jPY1WmdcQa8 >}}

Topics: 

- remember to log in your Google account. Open the tutorial code by clicking on the link above. Alternatively, in the video we explain how to choose a tutorial from GitHub. After the code is opened, remember to click on the "copy to drive" button
- wikipedia explanation:   https://en.wikipedia.org/wiki/Maurer_rose#Explanation
- the code on wikipedia is incomplete. Our tutorial includes a better version (wikipedia code only accepts a few values of 'd'. And our 'rose' version 2, which you can find at the end of the tutorial code, can draw also roses with 6, 10, 14, etc. petals)
- in the video above, we code the Maurer rose as a curve  (case in which 'd' equals 1)
- ...then we code drawing a Maurer rose without the curve: as a group of lines (cases in which 'd' is greater than 1)
- we show how to draw a Maurer rose with both the curve and lines
- we create videos by changing the number of petals and the parameter 'd'
- at the end of the code above, there is a version 2 of the Maurer rose code. It is not explained in the video. It allows for roses with all petal numbers, including 6, 12, 14, however the number of petals in version 2 must be an integer number


Kodindonesia provides a helper library, '**kicolab**', to make it easier to start having fun in Colab:

- the manual shows examples of almost all the features about how to use the canvas to draw and to display drawings
- there is also an advanced example on how to use kicolab with openCV. An upcoming video tutorial will show more openCV examples
- the manual above also shows how to create your own videos



