### PatchTST

PatchTST is a new transformer-based model that achieves state-of-the-art results for long-term forecasting tasks.

As the name suggests, it makes use of patching and of the transformer architecture. It also includes channel-independence to treat multivariate time series. The general architecture is shown below.



<img width="647" alt="PatchTST" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/bdaa4244-b127-44fb-946e-cf24513a474d">

*The PatchTST model architecture. We see that the model makes use of channel-independence to treat multivariate time series. In the transformer backbone, we also see the use of patching (illustrated by the rectangles). Plus, there are two versions to the model: supervised and self-supervised.*

### Channel-independence
Here, a multivariate time series is considered as a multi-channel signal. Each time series is basically a channel containing a signal.

<img width="647" alt="patchTST-2" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/4783175f-bfe0-4e3b-9a96-c1efbd341282">

In the figure above, we see how a multivariate time series is separated into individual series, and each is fed to the Transformer backbone as an input token. Then, predictions are made for each series and the results are concatenated for the final predictions.

### Patching
Most work on Transformer-based forecasting models focused on building new mechanisms to simplify the original attention mechanism. However, they still relied on point-wise attention, which is not ideal when it comes to time series.

In time series forecasting, we want to extract relationships between past time steps and future time steps to make predictions. With point-wise attention, we are trying to retrieve information from a single time step, without looking at what surrounds that point. In other words, we isolate a time step, and do not look at points before or after.

This is like trying to understand the meaning of a word without looking at the words around it in a sentence.

Therefore, PatchTST makes use of patching to extract local semantic information in time series.

### How patching works
Each input series is divided into patches, which are simply shorter series coming from the original one.
<img width="647" alt="patchTST-3" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/600c8407-f33a-48d6-8794-fe6711ff9ca8">

Here, the patch can be overlapping or non-overlapping. The number of patches depends on the length of the patch P and the stride S. Here, the stride is like in convolution, it is simply how many timesteps separate the beginning of consecutive patches.

<img width="647" alt="patchTST-4" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/8974def8-3473-4ce7-ae4f-4d273524c9eb">
In the figure above, we can visualize the result of patching. Here, we have a sequence length (L) of 15 time steps, with a patch length (P) of 5 and a stride (S) of 5. The result is the series being separated into 3 patches.

### Advantages of patching
With patching, the model can extract local semantic meaning by looking at groups of time steps, instead of looking at a single time step.

It also has the added benefit of greatly reducing the number of token being fed to the transformer encoder. Here, each patch becomes an input token to be input to the Transformer. That way, we can reduce the number of token from L to approximately L/S.

That way, we greatly reduce the space and time complexity of the model. This in turn means that we can feed the model a longer input sequence to extract meaningful temporal relationships.

Therefore, with patching, the model is faster, lighter, and can treat a longer input sequence, meaning that it can potentially learn more about the series and make better forecasts.

### Transformer encoder
Once the series is patched, it is then fed to the transformer encoder. This is the classical transformer architecture. Nothing was modified.

Then, the output is fed to linear layer, and predictions are made.


### Conclusion
PatchTST is a Transformer-based models that uses patching to extract local semantic meaning in time series data. This allows the model to be faster to train and to have a longer input window.

It has achieved state-of-the-art performances when compared to other Transformer-based models. In the code we saw that it also achieved better performances than N-BEATS and N-HiTS.

While this does not mean that it is better than N-HiTS or N-BEATS, it remains an interesting option when forecasting on a long horizon.
