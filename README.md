# Principle Component Analysis (PCA) for Return Allocation
## Key Concepts of PCA
PCA is a statistical factor model that identifies the underlying variance structure from high dimensional datasets. PCA is primarily a method for dimensionality reduction,
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
