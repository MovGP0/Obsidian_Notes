## Plots

- Timeline Plot
- Rug Plot
- Histogram
- Density Plot
- Density Waterfall Plots
- Box Plot (ie. Quartiles)

## Metrics

- Min, Max, Range
- Avergage
- Median
- Quantiles, Quartiles, Percentiles
- Modal Value
- Variance and Standard Deviation
- Skewness
- Kurtosis

## Number of measurements

$$ n = count(x) $$

## Average (arithmetic Mean)

$$ \bar{x} = \frac{ \sum_i x_i }{ n } $$

## Modal Value

Describes locations with a high density of results.

$$ h_m = \frac{ \sum_{i=1}^{len(h)} \left| h_i - h_{i-1} \right| }{ \max(h) } $$

## Interquartile Range (IQR)

Uses Qartiles to eliminate Outliers (Turkish Fence)

$$IRQ = Q3 - Q1$$
$$ \text{min value} = Q1 - 1.5\,IRQ$$
$$ \text{max value} = Q3 + 1.5\,IRQ$$

## Variance and Standard Deviation

Biased Variance
$$ s^2 = \frac{ \sum_i ( x_i - \bar{x} )^2 }{ n } $$

Biased Standard Deviation
$$ s = \sqrt{\frac{ \sum_i (x_i - \bar{x})^2 }{ n }}$$

Unbiased Variance
$$ s^2 = \frac{ \sum_i ( x_i - \bar{x} )^2 }{ n - 1 } $$

Unbiased Standard Deviation
$$ s = \sqrt{\frac{ \sum_i ( x_i - \bar{x} )^2 }{ n - 1 }} $$

## Skewness

Measure of asymmetry of measurements
$$ \gamma = \frac{ (x_n - \bar{x})^3 }{ n\,s^3 } $$

Pearson median shewness
$$ \gamma_\text{median} = \frac{ 3 ( \bar{x} - Q_2 ) }{ s } $$

## Kurtosis

Measure of peaketness
$$ \kappa = \frac{ \sum_i (x_n - \bar{x})^4 }{ n\,s^4 } $$

### Excess Kurtosis

Kurtosis of Normal Distribution is 3. When subtracting we get the excess.

$$ \kappa = \frac{ \sum_i (x_n - \bar{x})^4 }{ n\,s^4 } - 3 $$

## Standard Error

$$ \frac{s}{\sqrt{n}} $$

## Margin of Error

$$ t(x) \frac{s}{\sqrt{n}} $$

where `t` ist a function describing the confidence based on the size of the sample. 

## Confidence Interval

$$ \left[ \bar{x} - t(x) \frac{s}{\sqrt{n}} \dots \bar{x} + t(x) \frac{s}{\sqrt{n}\right] $$
