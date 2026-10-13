<!-- solve-problem:4e101aea-0db9-4113-8877-1f7d008ce8ac -->
# Joint vault-and-holder peg equilibrium

A [Tangle Solve problem](https://solve.tangle.tools/problems/4e101aea-0db9-4113-8877-1f7d008ce8ac).

## What is the problem?

Formalize a stablecoin system with two strategic layers:

**State variables:** Let the stablecoin price be p_t relative to peg (target p* = 1). A noncustodial/collateralized system (e.g., a vault-based design like Dai) has n vaults; vault i chooses collateral ratio (leverage) c_i,t and issuance q_i,t. Holders form a continuum (or finite population) with heterogeneous outside options/redemption costs θ; each period a holder chooses {hold, redeem, exit to alternative currency}.

**Dynamics:** Vault leverage decisions feed aggregate supply and collateralization of the peg; holder redemption decisions determine demand and price pressure; realized p_t feeds back into vault solvency and liquidations, and into holder incentives to redeem (run behavior).

**Question:** Characterize the joint equilibrium — a fixed point in which vaults' leverage policies and holders' entry/exit/redeem policies are mutually optimal given the resulting price dynamics — and determine under what parameter regimes the peg is (i) stable in a self-fulfilling sense (no profitable deviation to redeem/exit, no incentive for vaults to over-lever), (ii) admits multiple equilibria including run equilibria, (iii) collapses. Deliver conditions (on collateral rules, fees, liquidation parameters, heterogeneity distribution of θ) characterizing the stability region, ideally as explicit thresholds.

**Ambiguity note:** The census statement is informal. Reasonable formalizations differ on whether time is discrete/continuous, whether the stochastic shock process is exogenous (as in Klages-Mundt–Minca) or partly endogenous, and whether holders are a continuum (mean-field) or finite (strategic atomic). State your choice explicitly.

## Why does it matter?

Existing stability results for noncustodial stablecoins (e.g., Klages-Mundt–Minca) assume exogenous/fixed demand and a representative holder; real systems have both strategic whales/vaults choosing leverage and users who can exit to competitors (USDC, fiat). A joint equilibrium characterization would tell designers which collateral and fee parameters prevent endogenous instability spirals (over-leverage → depeg → run → forced liquidation). It connects directly to DeFi protocol design (Maker/Sky, Liquity), central-bank concerns about stablecoin runs, and the systemic-risk literature on coordination failure.

## What's already known?

- Klages-Mundt and Minca (2022) analyze a stochastic model of noncustodial stablecoins with exogenous demand, deriving stable and unstable regimes via a Lyapunov/stability analysis of the price and vault-incentive dynamics. Crucially, holders do not strategically choose participation; demand is a stochastic process, not a decision.
- The banking-run literature (global games, à la Goldstein–Pauzner; Morris–Shin refinements) gives conditions under which runs are uniquely determined vs. multiple equilibria, but for fixed bank balance sheets, not endogenous vault leverage.
- Mean-field-game tools with entry/exit exist (e.g., in oligopoly and bank-run mean-field models, "mean-field games with entry" cited in the census) but have not been applied to joint vault-leverage + holder-exit fixed points. (verify specific prior applications)
- Interacting-bank systemic risk models (e.g., Eisenberg–Noe-style clearing, debt cascades) cover one direction of the feedback (asset fire sales ↔ solvency) but not peg price dynamics.
- Gap: no published result closes the loop between strategic leverage supply of stablecoins and strategic demand (entry/redemption/switching).

## What would count as solving it?

1. **Model existence:** Prove existence of a joint equilibrium (vault leverage policies + holder entry/exit policies) in a clearly specified stochastic dynamic game.
2. **Stability characterization:** Derive explicit sufficient (ideally necessary-and-sufficient) conditions on primitives — collateral ratio floor, stability fee, liquidation penalty, distribution of holder outside options θ, shock volatility — such that the peg is locally (or globally) stable in the resulting equilibrium.
3. **Run/multiplicity result:** Either prove uniqueness of the non-run equilibrium under some refinement (global-games noise), or construct explicit run equilibria and bound the stability region boundary.
4. **Optional comparative statics/design corollaries:** e.g., monotone effect of liquidation penalty on the stable region; threshold θ* such that holders with outside option above θ* exit.

A referee accepts: a formal theorem with stated assumptions, not simulation alone (though simulations supporting a conjectured threshold are valuable partial progress).

## How to start

1. **Read Klages-Mundt–Minca (2022) Sections on regime characterization first** — adopt their stochastic price/collateral dynamics as the backbone and identify exactly where fixed demand enters; this is the piece to endogenize.
2. **Replace representative demand with a two-type or continuum holder exit decision**, solved first for *given* vault policies as a global-games/coordination problem; this gives a redemption-demand correspondence D(p, θ-distribution).
3. **Close the loop:** treat vaults as choosing leverage against anticipated D(·); the fixed point can be attacked via mean-field-game fixed-point theorems (Carmona–Delarue-style) or, in a two-period version, via standard subgame-perfect analysis.
4. **Main obstacle:** the run region and the over-leverage incentive are complements — vaults lever more when demand is sticky, but leverage widens the run region — so naive fixed-point iteration may cycle; expect to work in a coarse action space (two leverage levels, two holder types) first.

## Key references

- Klages-Mundt, A., & Minca, A. (2022). "While stability lasts: A stochastic model of noncustodial stablecoins." *Mathematical Finance*. DOI:10.1111/mafi.12357 — Stochastic model of a noncustodial stablecoin with vault liquidations, characterizing stable vs. unstable parameter regimes under exogenous demand; the direct predecessor and baseline to extend. https://doi.org/10.1111/mafi.12357
- Goldstein, I., & Pauzner, A. (2005). "Demand–deposit contracts with an unknown number of depositors." *Journal of Economic Theory*. (verify exact pairing) — Global-games uniqueness of bank-run equilibria; the natural toolkit for the holder-exit layer.
- Carmona, R., & Delarue, F. — *Probabilistic Theory of Mean Field Games* (vols. I–II, Springer). — Standard existence/uniqueness machinery for continuum-population games; candidate tool for the holder layer. (verify edition)
- Eisenberg, L., & Noe, T. (2001). "Systemic risk in financial systems." *Management Science*. — Clearing-system framework for cascading defaults; relevant to the liquidation feedback channel.

## Warnings

- The census entry is a one-line research target, not a formal problem; any precise statement you adopt is a modeling choice — justify it and check robustness to discrete vs. continuous time.
- Do not assume fixed demand "for convenience" — that silently reduces the problem to the already-solved Klages-Mundt–Minca regime analysis.
- Beware trivial equilibria: if holders all exit or all vaults refuse to mint, the model can have degenerate equilibria; specify participation/entry assumptions up front.
- Endogenizing the shock process itself (e.g., making volatility depend on redemptions) can make existence proofs much harder — keep the shock exogenous initially.
- Related DeFi-empirics papers about depegs are not theory substitutes; the census asks for equilibrium characterization.

---
*`BCGT-000114` · flagged for review*

## Acceptance guidance

1. **Model existence:** Prove existence of a joint equilibrium (vault leverage policies + holder entry/exit policies) in a clearly specified stochastic dynamic game.
2. **Stability characterization:** Derive explicit sufficient (ideally necessary-and-sufficient) conditions on primitives — collateral ratio floor, stability fee, liquidation penalty, distribution of holder outside options θ, shock volatility — such that the peg is locally (or globally) stable in the resulting equilibrium.
3. **Run/multiplicity result:** Either prove uniqueness of the non-run equilibrium under some refinement (global-games noise), or construct explicit run equilibria and bound the stability region boundary.
4. **Optional comparative statics/design corollaries:** e.g., monotone effect of liquidation penalty on the stable region; threshold θ* such that holders with outside option above θ* exit.

A referee accepts: a formal theorem with stated assumptions, not simulation alone (though simulations supporting a conjectured threshold are valuable partial progress).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/4e101aea-0db9-4113-8877-1f7d008ce8ac?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000114.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000114)
