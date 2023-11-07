### N-HiTS 

**N-HiTS stands for Neural Hierarchical interpolation for Time Series forecasting.**

In short, N-HiTS is an extension of the N-BEATS model that improves the accuracy of the predictions and reduces the computational cost. This is achieved by the model sampling the time series at different rates. That way, the model can learn short-term and long-term effects in the series. Then, when generating the predictions, it will *combine the forecasts made at different time scales, considering both long-term and short-term effects.* This is called hierarchical interpolation.

The architecture of N-HiTS:


<img width="647" alt="N-HiTS" src="https://github.com/aussiekom/Data-Science-Projects/assets/102028836/6ad6621b-bbe6-4adb-bd69-66eb958cf29d">
