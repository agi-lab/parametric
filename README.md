# When Indemnity Insurance Fails

## Parametric Coverage under Binding Budget and Risk Constraints

This repository contains the code to replicate the numerical illustrations in the paper

> **When Indemnity Insurance Fails: Parametric Coverage under Binding Budget and Risk Constraints**\
> Benjamin Avanzi, Debbie Kusch Falden, and Mogens Steffensen\
> *Journal of Risk and Insurance* (2026), 1–32. <https://dx.doi.org/10.1111/jori.70070>

In high-risk environments, traditional indemnity insurance is often unaffordable or ineffective, despite its well-known optimality under expected utility. We compare excess-of-loss indemnity insurance with parametric insurance within a common mean-variance framework, allowing for fixed costs, heterogeneous premium loadings, and binding budget constraints. Motivated by the disaster insurance and risk-sharing literature, we show that, once these realistic frictions are introduced, parametric insurance can yield higher welfare for risk-averse individuals, even under the same utility objective and without relying on behavioral assumptions. The welfare advantage arises precisely when indemnity insurance becomes impractical (particularly when households face binding premium budgets), and disappears once both contracts are unconstrained. Our results help reconcile classical insurance theory with the growing use of parametric risk transfer in high-risk settings, and rationalize the interest in hybrid designs that combine both indemnity and parametric elements.

------------------------------------------------------------------------

## html version

An rendered html version of the Rmd can be seen by clicking this link:

👉 [**https://agi-lab.github.io/parametric/**](https://agi-lab.github.io/parametric/){.uri}

> **Note:** Clicking `.html` files directly inside GitHub will only show the source code. Use the link above to view the rendered code.

------------------------------------------------------------------------

### Correction, September 2026: the loading indifference threshold in Figure 2

One number reported in Section 4 of the paper is inconsistent with the figure it annotates. The error is in the replication code, is confined to a single quantity, and does not affect any conclusion. It has been corrected in `AvFaSt25-numerics.Rmd`; this note records what it was, why it happened, and what changes.

#### What was wrong

Section 4 states that the indemnity loading at which the agent becomes indifferent between the two covers is

> $\theta_d = 1.29$ without premium matching, and $1.57$ with premium matching,

in a passage that also specifies $\theta_p = 0.2$ for that figure.

The two do not go together. At $\theta_p = 0.2$ the unmatched threshold is **1.161**, not 1.29. The value 1.29 is the threshold at $\theta_p = 0.3$.

The consequence is visible in the published Figure 2(a) (`fig_theta_1d`): the green $\mathrm{MV}^{(d)}$ curve crosses the black $\mathrm{MV}^{(p)}$ line at about $\theta_d = 1.16$, but the dashed $\theta_{\mathrm{indif}}$ marker is drawn at 1.29, to the right of the crossing it is meant to mark. The second marker, $\theta'_{\mathrm{indif}}$ at 1.57, sits correctly on its crossing.

#### Why it happened

An ordering problem between chunks, not an error in the mathematics.

`theta_p` is set to `0.3` in the `premiumpara` chunk, which is the default used by Figure 1. The `thetaindif` chunk then computes both thresholds. Only afterwards did the `new theta` chunk set `theta_p <- 0.2` for Figure 2. The plot draws its vertical markers from the roots computed in the earlier chunk, so the markers carried $\theta_p = 0.3$ onto a figure built at $\theta_p = 0.2$.

The premium-matched threshold escaped almost unchanged because it is nearly insensitive to $\theta_p$: matching the parametric premium to the indemnity premium largely removes the dependence.

#### The corrected values

At $\theta_p = 0.2$, with $\gamma_d = \gamma_p = 0$:

| Quantity                                    | Published | Corrected |
|---------------------------------------------|-----------|-----------|
| $\theta_{\mathrm{indif}}$, unmatched        | 1.29      | **1.161** |
| $\theta'_{\mathrm{indif}}$, premium-matched | 1.57      | **1.573** |

At $\theta_p = 0.3$ the two thresholds are 1.294 and 1.570, which is where the published numbers come from.

#### What it does and does not affect

**It does not affect any conclusion, and the corrected number strengthens the result.** The threshold moves *down*, so the region of the parameter space in which parametric insurance dominates is larger than the paper reports, not smaller. The empirical comparison in the same passage is unchanged: the median loss ratio of 0.28 reported by Jung and Jung (2026) for US homeowner's insurance corresponds to $\theta_d \approx 2.6$, far above both 1.16 and 1.29.

Nothing else in the paper is touched. In particular the following were re-derived independently and reproduce the published values exactly:

- $\mathbb{E}[Y_i] = 266{,}122$, $\Pr[Y_i = L] = 24\%$
- $d^{*} = 22{,}500$, $k^{*} = 243{,}622$, and the duality $d^{*} + k^{*} = \mathbb{E}[Y_i]$
- premiums at the optima, $6{,}353$ and $6{,}334$
- the no-insurance benchmark $\mathrm{MV}^{(0)} = 131{,}023$
- the fixed-cost thresholds $\gamma_{\mathrm{indif}} = 3{,}239$ and $\gamma'_{\mathrm{indif}} = 9{,}980$
- the budget-constrained results underlying Figure 3
- the premium-matched loading threshold, $1.57$

Figures 1 and 3 are unaffected: both are produced with $\theta_d = \theta_p = 0.3$, set explicitly in their own chunks. Figure 2(b), the $(\theta_d, \gamma_d)$ surface, is unaffected because it is drawn after `theta_p <- 0.2` and does not use the thresholds.

#### The fix

`theta_p <- 0.2` is now set at the top of the `thetaindif` chunk, so the thresholds and the figure they annotate share the same $\theta_p$. The change is a single assignment, annotated in place.

Re-running the notebook regenerates Figure 2(a) with the marker at 1.161, on the crossing.
