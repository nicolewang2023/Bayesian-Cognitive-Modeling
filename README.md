# Bayesian-Cognitive-Modeling

This project implements a custom Metropolis MCMC sampler to estimate a latent psychological parameter from behavioral task data using Bayesian inference. The parameter of interest, **W**, represents cognitive noise in perceptual decision-making. We analyze real-world psychophysics data and visualize the posterior distribution of W, comparing it to the prior and exploring its implications.

## Project Overview

In cognitive science, understanding how people make decisions under uncertainty is fundamental. A key variable in many psychophysical models is **W**, which governs perceptual noise when comparing two stimuli. Using observed accuracy from a same-versus-different task (`a`) and the corresponding stimuli (`n1`, `n2`), we use a Bayesian approach to infer a plausible posterior distribution over W.

## Dataset

Each row of the dataset includes:

| Column | Description |
|--------|-------------|
| `a`    | 1 if correct response, 0 otherwise |
| `n1`   | Stimulus 1 magnitude |
| `n2`   | Stimulus 2 magnitude |

There are approximately 3,500 rows.

## Methodology

### Model

We define:

- **Prior**: Exponential decay — `P(W) ∝ exp(-W)` for `W > 0`
- **Likelihood**: Based on a psychometric function, the probability of a correct response is modeled as:

  \[
  P(\text{correct}) = 1 - \Phi\left( \frac{0 - |n1 - n2|}{W \cdot \sqrt{n1^2 + n2^2}} \right)
  \]

- **Posterior**: Calculated as the sum of log-prior and log-likelihood values:  
  `log_posterior(W) = log_prior(W) + log_likelihood(W)`

### Inference

We implement the Metropolis algorithm to sample from the posterior distribution:

- Burn-in: First 1,000 samples discarded
- Total iterations: 10,000
- Proposal distribution: Normal(0, 0.1)

## Results

Key analysis outputs include:

- Trace plot of W over iterations
- Histogram of posterior samples
- Prior vs. posterior comparison
- Estimated probability that W falls within a user-defined interval (e.g., `[0.60, 0.65]`)

Summary statistics:

| Metric                            | Value        |
|----------------------------------|--------------|
| Posterior mean of W              | ~0.62        |
| 95% credible interval            | ~[0.57, 0.68] |
| Probability W ∈ [0.60, 0.65]     | ~24.5%       |

## Project Structure

- `data.csv`: Input dataset
- `main.py`: Full pipeline — data loading, posterior sampling, visualizations
- `README.md`: Project documentation

## Dependencies

Install required libraries with:

```bash
pip install numpy pandas matplotlib scipy
