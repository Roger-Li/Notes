Chapter 13 Sequential Data
==========================
### 13.1 Markov Models
* (pp. 609) Higher order Markov models have exponentially growing number of parameters ($K^{M-1}(K-1)$, where $K$ is the number of state and $M$ is the order). This is the motivation of introducing additional latent variables to "permit a rich class of models to be constructed out of simple components."

  This is insured by conditional independence of latent variables $\mathbf{z}_{n+1} \perp \mathbf{z}_{n-1} | \mathbf{z}_{n} $, using the d-separation criterion (pp. 379), we see that there is always a path connecting any two observed variables $\mathbf{x_n}$ and $\mathbf{x_m}$.

  *（the reading is halted at this point）*
