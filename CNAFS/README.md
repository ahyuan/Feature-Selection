# CNAFS

MATLAB implementation of **Convex Non-Negative Matrix Factorization With Adaptive Graph for Unsupervised Feature Selection**.

CNAFS is an unsupervised feature selection method that jointly learns a convex non-negative matrix factorization, an adaptive similarity graph, and a pseudo-label matrix. After optimization, features are ranked by the row-wise score of the learned projection matrix `W`; the top-ranked indices are returned for downstream clustering or classification.

## Paper

- **Title:** Convex Non-Negative Matrix Factorization With Adaptive Graph for Unsupervised Feature Selection
- **Authors:** Aihong Yuan, Mengbo You, Dongjian He, and Xuelong Li
- **Journal:** IEEE Transactions on Cybernetics, vol. 52, no. 6, pp. 5522-5534, 2022
- **DOI:** [10.1109/TCYB.2020.3034462](https://doi.org/10.1109/TCYB.2020.3034462)
- **IEEE Xplore:** [document 9271922](https://ieeexplore.ieee.org/document/9271922/)

## Repository Contents

- `CNAFS.m` - core MATLAB function for CNAFS optimization and feature ranking.
- `README.md` - project description, usage notes, results summary, and citation.

## Requirements

- MATLAB.
- A `gpi.m` implementation on the MATLAB path. `CNAFS.m` calls `gpi` for the generalized power iteration step referenced in the paper:
  - F. Nie, R. Zhang, and X. Li, "A generalized power iteration method for solving quadratic problem on the Stiefel manifold," Science China Information Sciences, 2017.
- Input data matrix `X` should be arranged as `d x n`, where `d` is the number of features and `n` is the number of samples.

The repository contains the core CNAFS implementation only; benchmark datasets and experiment scripts are not included.

## Usage

```matlab
% X: d-by-n data matrix, with samples stored as columns
% c: number of clusters/classes expected in the data
c = 10;
alpha = 1;
beta = 1;
lambda = 1;
gamma = 1;
epsilon = 1e-6;
NITER = 1000;
NMF_K = c;

[W, score, index, objectives] = CNAFS(X, c, alpha, beta, lambda, gamma, epsilon, NITER, NMF_K);

num_features = 100;
selected_feature_index = index(1:num_features);
X_selected = X(selected_feature_index, :);
```

### Function Signature

```matlab
[W, score, index, objectives] = CNAFS(X, c, alpha, beta, lambda, gamma, epsilon, NITER, NMF_K)
```

| Argument | Description |
| --- | --- |
| `X` | `d x n` data matrix; each column is one sample. |
| `c` | Desired cluster number. |
| `alpha`, `beta`, `lambda`, `gamma`, `epsilon` | Model hyperparameters described in the paper. |
| `NITER` | Maximum number of optimization iterations. |
| `NMF_K` | Number of basis vectors for convex non-negative matrix factorization. |

| Output | Description |
| --- | --- |
| `W` | `d x c` projection matrix. |
| `score` | Feature score vector computed from `W`. |
| `index` | Feature indices sorted by descending score. |
| `objectives` | Objective values across iterations. |

## Experimental Results

The paper evaluates feature selection by running K-means on the selected features. The table below summarizes the CNAFS rows reported in Tables II and III. Larger values are better.

| Dataset | ACC (% +/- std) | NMI (% +/- std) |
| --- | ---: | ---: |
| MNIST | 58.64 +/- 1.60 | 49.34 +/- 1.60 |
| warpAR10P | 44.23 +/- 3.62 | 44.51 +/- 3.32 |
| warpPIE10P | 55.24 +/- 2.42 | 59.35 +/- 3.67 |
| madelon | 60.32 +/- 0.08 | 59.28 +/- 0.05 |
| COIL20 | 68.33 +/- 3.86 | 77.72 +/- 2.81 |
| JAFFE | 79.81 +/- 6.08 | 85.05 +/- 2.84 |
| Orlraws | 81.70 +/- 3.92 | 79.21 +/- 2.77 |
| USPS | 71.65 +/- 1.69 | 61.91 +/- 1.45 |

In the reported benchmark comparison, CNAFS obtains the best ACC on all eight datasets. For NMI, CNAFS obtains the best result on six datasets, ranks second on USPS, and ranks third on Orlraws.

## Citation

Please cite the paper if this code is useful for your research:

```bibtex
@ARTICLE{9271922,
  author={Yuan, Aihong and You, Mengbo and He, Dongjian and Li, Xuelong},
  journal={IEEE Transactions on Cybernetics},
  title={Convex Non-Negative Matrix Factorization With Adaptive Graph for Unsupervised Feature Selection},
  year={2022},
  volume={52},
  number={6},
  pages={5522-5534},
  doi={10.1109/TCYB.2020.3034462}
}
```
