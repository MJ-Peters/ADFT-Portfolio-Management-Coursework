# Principle Component Analysis (PCA) for Return Allocation
## Key Concepts of PCA (Applied to the S&P 500)
PCA is a statistical factor model that identitfies the underlying variance structure from high dimensional datasets. PCA is primarily a method for dimensionality reduction,
allowing for the reduction of a large number of assets into a smaller set of principal components, the eigenvectors, for the covariance matrix that capture most of the covariation
among all assets. This is why PCA is so widely used in data analysis; it allows the user to reduce the scale of their dataset and simplify the analysis while simultaneously identifying
the features and variables within the data. In the context of portfolio management, PCA will help investors identify key assets within a large dataset (such as the S&P 500) and
allocate capital based on the output of the analysis. Though this is very useful, investments must not be based on PCA alone, as low-quality data could lead to the misidentification
of principal components and the resultant construction of a less-than-adequate portfolio. The user must also consider how to spread capital as PCA, unlike hierarchical risk parity (HRP),
does not assign weights to individual assets for the purpose of capital allocation, though the eigenvectors that PCA produces can be converted to portfolio weights. The key steps of PCA
are as follows:

### Data Standardisation
The first step in PCA is to standardize the data by subtracting the mean of each variable and dividing by the standard deviation of the set. This ensures the dataset is centered on the
origin and makes the calculation of the covariance matrix simpler. This is described in the equation below where $z_{ij}$ is the standardized value of the $i$th observation on the $j$th variable,
$x_{ij}$ is the original value, $\mu_j$ is the mean of the $j$th variable, and $\sigma_j$ is the standard deviation of the $j$th variable.

$$
z_{ij} = \frac{x_{ij} - \mu_j}{\sigma_j}
$$

### Covariance Matrix Calculation
The second step is to calculate the covariance matrix of the standardized data, which represents the variance between the different variables in the data and not itself. This is described
in the equation below where $z_{ik}$ and $z_{jk}$ are the $i$-th and $j$-th variables of the $k$-th observation, and $\bar{z}_i$ and $\bar{z}_j$ are the means of the $i$-th and $j$-th
variables, respectively.

$$
\Theta(i, j) = \frac{1}{n} \sum_{k=1}^{n}(z_{ik}-\bar{z}_i)(z_{jk}-\bar{z}_j)
$$

### Eigenvalue and Eigenvector Calculation
The third step is to calculate the eigenvectors and eigenvalues of the covariance matrix we have previously calculated. The eigenvectors represent the directions of the maximum variance
(principal components) in the data, and the eigenvalues represent the amount of variance in these directions. This is described in the equation below where $\mathbf{v}$ is the eigenvector,
and $\lambda$ is the corresponding eigenvalue.

$$
\Theta\mathbf{v} = \lambda\mathbf{v}
$$

### Principle Component Selection
The fourth step is to select the principal components. This is achieved by sorting the eigenvalues, and their eigenvectors, in descending order. Top $k$ eigenvalues are chosen, as they
explain the most variance in the data, and their corresponding eigenvectors are the selected principal components. To determine $k$, the cumulative proportion of variance explained by
each principal component needs to be examined, thus, $k$ depends on the total amount of variance that needs to be explained in the data.

### Dimensionality Reduction
The final step is to project the standardized data onto a new coordinate system defined by the principle components. This gives a set of variables that captures the majority of
information and variance from the original dataset but at a much lower dimensionality, for instance it could distill all 503 S&P 500 stocks into 40/50 key assets that are responsible for
the majority of variance in the dataset. This is described in the equation below where $y_i$ is the $i$th observation in the new coordinate system, $\mathbf{v}$ is the eigenvector matrix,
and $z_i$ is the standardized vector of the $i$th observation.

$$
y_i = \mathbf{v}^Tz_i
$$


## Predicting the Allocation Weight of Dow Jones Industrial Average (DJIA) Index Stocks
### Data Cleansing
The first step I took in handling missing values was the removal of features (stocks) that had NaN values for more than 30\% of their total data, as this would be equivalent to approximately
six years of missing data. This resulted in the removal of DWDP and V (Visa) from the dataset, as their first data entries were in 2017 ($\approx17$ years missing data) and 2008 ($\approx8$
years missing data), respectively. Following this operation, I had to handle any other NaN values other features may have. I applied a forward fill method to the data frame so that any
remaining NaN values would be replaced by the last close price value available for a given feature. This does not work on data where there is no previously available data; in these cases,
the NaN values were dropped from the data frame altogether with no fill value.

### Cumulative Variance
The cumulative explained variance plot, which can be seen in the jupyter notebook, shows the top 15 factors in the DJIA data set. The plot shows that the most significant factor, factor 0,
is responsible for about 40\% of the explained variance for the whole data set, while the top 10 factors are responsible for about 70\% of the explained variance and the top 15 factors are
responsible for about 80\% of the explained variance. This shows that the vast majority of the variance in the set is explained by only half of the total factors in the dataset.

### Weight Allocation
As I mentioned previously, PCA itself does not provide weightings for the allocation of capital in the context of portfolio management, but the eigenvectors of principal components it
does produce can be converted for this purpose. First, the components of the PCA are normalised by dividing each component within a portfolio by the sum of all components in that portfolio.
These values are then transposed and used as weights for the allocation of capital in a portfolio, where a negative weight indicates you should hold a short position, a positive weight
indicates a long position, and the magnitude of the weight is the proportion of capital that should be allocated.

### Evaluation
The Sharpe ratio, with respect to portfolio management, is the measure of a portfolio’s risk-return trade-off, equal to the portfolio’s risk premium divided by its volatility. This is
described in the equation below where $E[R_p]$ is the expected return of the portfolio, $R_f$ is the risk free rate of return, and $\sigma_p$ is the standard deviation of the portfolio
(i.e. its risk/volatility):

$$
Sharpe\ Ratio = \frac{E[R_p]R_f}{\sigma_p}
$$

It measures the excess return of the portfolio per unit of risk, where a unit of risk is the standard deviation of returns. A high Sharpe ratio indicates that the portfolio has a good
risk-adjusted return, i.e. there is not a large excess of risk to achieve its return. In a series of portfolios that have been generated, the tangency portfolio is the portfolio that
has the highest possible Sharpe ratio of any of the other portfolios. In this analysis, this was determined to be portfolio 0 with a Sharpe ratio of 0.86, return of 11.51\%, and
volatility of 13.44\%. The Sharpe ratios for all eigenportfolios generated can be seen in the jupyter notebook.

I then took the top three eigenportfolios (i.e. the three with the highest Sharpe Ratios: 0, 10, and 5) and compared their performance against the equal weighted index using unseen
testing data and plotted the results. When applied to the raw test data, portfolio 0 had a return of 22.96\%, a volatility of 11.35\%, and a Sharpe ratio of 2.02. This is very similar
to the equal weight index fund, which had a return of 22.99\%, a volatility of 11.19\%, and a Sharpe ratio of 2.05. Although in training portfolio 0 was the best in training, it was
outperformed by an equal weight index and would, therefore, be considered ineffective.

![Portfolio 0](https://github.com/MJ-Peters/DJ-and-FTSE-Portfolio-Management/blob/master/images/portfolio%201%20(0).png)

Eigenportfolio 10, the second best eigenportfolio in training, was significantly outperformed by the equally weighted index fund in practice. It had a return of -18.84\%, a volatility
of 67.71\%, and a Sharpe ratio of -0.28.

![Portfolio 10](https://github.com/MJ-Peters/DJ-and-FTSE-Portfolio-Management/blob/master/images/portfolio%202%20(10).png)

eigenportfolio 5, the third best eigenportfolio in training, was by far the best out of all three eigenportfolios considered. It had a return of 188.84\%, a volatility of 93.05\%, and
a Sharpe ratio of 2.03. All this considered, it still had a lower Sharpe ratio than the equally weighted index fund meaning that, despite it having a huge amount of return in hindsight,
without the future knowledge that this portfolio would perform well the portfolio that best maximises risk-adjusted returns was the equal weight index.

![Portfolio 5](https://github.com/MJ-Peters/DJ-and-FTSE-Portfolio-Management/blob/master/images/portfolio%203%20(5).png)

# Hierarchical Risk Parity

