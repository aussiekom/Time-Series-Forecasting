### TimesNet
The motivation behind TimesNet comes from the realization that many real-life time series exhibit mutli-periodicity. This means that variations occur at different periods.

*For example, temperature outside has both a daily and a yearly period. Usually, it is hotter during the day than at night, and hotter during summer than in winter.*

Now, those multiple periods overlap and interact with each other, making it difficult to separate and model individually.

So, the authors of TimesNet propose to **reshape the series in a 2D space to model intraperiod-variation and interperiod-variation.**

Going back to our weather example, intraperiod-variation would be how the temperature varies within a day, and interperiod-variation would be how the temperature varies from day to day, or from year to year.

### The architecture of TimesNet:
<img width="647" alt="timesnet-1" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/8031040f-86c4-4056-8aa2-f225911c9c25">

From the figure above, we can see that TimesNet is a stack of multiple TimesBlock with residual connections.

In each TimesBlock, we can see that the series first goes through a fast Fourier transform (FTT) to find the different periods in the data. Then, it is reshaped as a 2D vector and sent to an Inception block where it learns and predicts a 2D representation of the series. Then, this deep representation must be reshaped back to a 1-dimensional vector using adaptive aggregation.

There is a lot to understand here, so let’s cover each step in more detail.

### Capture multi-periodicity
To capture the variations across multiple periods in time series, the authors suggest to transform the 1-dimensional series into a 2-dimensional space to simultaneously model intraperiod and interperiod variations.
<img width="647" alt="timesnet-2" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/fb0cb9ff-6483-436e-ac60-cb35b2af4841">

In the figure above, we can see how the model represents the variations in a 2D space. Inside the red rectangles, we can see the intraperiod-variation, which is how the data changes inside a period. Then, the blue rectangle contains the interperiod-variation, which is how the data changes from period to period.

To better understand that, suppose that we have daily data with a weekly period. The interperiod-variation is how the data changes from Monday, to Tuesday, to Wednesday, and so on.

Then, the interperiod-variation would be how the data varies from Monday in week 1, to Monday in week 2, and from Tuesday in week 1, to Tuesday in week 2. In other words, it is the variation of data in the same phase at different periods.

Those variations are then represented in a 2D space, where the interperiod-variation is vertical, and the intraperiod-variation is horizontal. This allows the model to learn a better representation of the variation in the data.

Whereas a 1-dimensional vector shows variations between adjacent points, this 2D representation shows variations between adjacent points and adjacent periods, giving a more complete picture of what is happening.

Yet, one question remains: how do we find the periods in our series?

### Identify the significant periods in a data
To identify the multiple periods in time series, the model applied the Fast Fourier Transform (FTT).

This is a mathematical operation that transforms a signal into a function of frequency and amplitude.
<img width="647" alt="timesnet-3" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/42847dc9-382d-48ed-ba7a-b31e79cdf0f9">

In the figure above, the authors illustrate how FTT is applied. Once we have the frequency and amplitude of each period, the ones with the largest amplitudes are considered to be the most relevant.

Once FTT is applied, the user can set a parameter k to select the top-k most important periods, which are the ones with the largest amplitude.

TimesNet then creates 2D vectors for each period, and those are sent to a 2D kernel to capture the temporal variations.

### Inside the TimesBlock
Once the series has gone through the Fourier transform and 2D tensors were created for the top-k periods, the data is sent to the Inception block, as shown in the figure below.
<img width="647" alt="timesnet-4" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/7a6f6591-4a28-4bf8-b457-938162334927">

Of course, note that all of the steps that we explored are carried out inside the TimesBlock.

Now, the 2D representation of the data is sent to the Inception block.

The Inception module is the building block of the computer vision model [GoogLeNet](https://openaccess.thecvf.com/content_cvpr_2015/papers/Szegedy_Going_Deeper_With_2015_CVPR_paper.pdf), published in 2015.

The main idea of the Inception module is to have an efficient representation of the data by keeping it sparse. That way, we can technically increase the size of a neural network, while keeping it computationally efficient.

This is achieved by performing various convolutions and pooling operations, and then concatenating everything. In the context of TimesNet, this is how the Inception module looks like.
<img width="647" alt="timesnet-5" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/f39c232d-a3f4-45e4-8f96-2ac227534c19">

You might wonder why the authors chose a vision model to treat time series data.

**A simple answer to that is that vision models are particularly good at parsing 2D data, like images.**

The other benefit is that the vision backbone can be changed inside TimesNet. While the authors use the Inception block, it can be changed to other vision model backbones, and so TimesNet can also benefit from the advances in computer vision.

Now, one element that separates the Inception module in TimesNet from the inception module in GoogLeNet is the use of adaptive aggregation.

### Adaptive aggregation
To perform aggregation, the 2D representation must be reshaped to 1D vectors first.

Then, adaptive aggregation is used, because different periods had different amplitudes, which indicates how important they are.

This is why the output of the FTT is also sent to a softmax layer, so that the aggregation is done using the relative important of each period.

The aggregated data is the output of a single TimesBlock. Then, multiple TimesBlock are stacked with residual connections to create the TimesNet model.

Now that we understand how the TimesNet model works, let’s test it on a forecasting task, alongside N-BEATS and N-HITS.






















