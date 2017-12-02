* We have the bayes rule as follows:
$$p(A|B)=\dfrac{p(B|A)P(A)}{P(B)}$$
If we don't have $p(B)$ directly, we can calculate it from the numerator, i.e.
$$P(B) = \int_A P(A,B) = \int_Ap(B|A)P(A)$$
* We can think of the bottom term as a "normalization constant" so that the distribution sums up to 1
$$p(A|B)=\dfrac{p(B|A)P(A)}{\int_Ap(B|A)P(A)} = \dfrac{p(B|A)P(A)}{Z} \propto p(B|A)P(A)$$
as $Z$ is independent of $A$. This could be useful if we want to optimize over $A$, i.e.,
$$\text{argmax}_A P(A|B) = \text{argmax}_A P(B|A) P(A)$$
