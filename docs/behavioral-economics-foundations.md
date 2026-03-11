# Behavioral Economics Foundations

The Styx platform operationalizes four decades of behavioral economics research into a single design thesis: humans systematically fail to follow through on their own intentions, and financial commitment devices calibrated to prospect theory can close the gap between intention and action. This document formalizes the theoretical foundations that justify every major design decision in the system.

## Prospect Theory and the Loss Aversion Coefficient

The entire staking mechanism rests on a single empirical regularity: the asymmetric weighting of losses relative to gains in human decision-making (Kahneman & Tversky, 1979). Prospect theory established that people evaluate outcomes relative to a reference point rather than absolute wealth levels, and that the value function is concave for gains, convex for losses, and approximately twice as steep for losses as for gains.

The loss aversion coefficient, denoted lambda, was originally measured at approximately 2.0 and later refined to lambda = 1.955 in cumulative prospect theory (Tversky & Kahneman, 1992). Styx adopts lambda = 1.955 as its calibration constant. This is not decorative. It means a user who stakes $50 experiences the potential loss with roughly the same psychological magnitude as a $98 reward. The staking mechanism exploits this asymmetry: the threat of losing one's own money motivates compliance approximately twice as powerfully as an equivalent bonus would.

The fourfold pattern of risk attitudes (Tversky & Kahneman, 1992) further informs tiered staking design. Micro-stakes users ($5) operate in a qualitatively different risk quadrant than users staking $100 or more. Small-probability-large-loss framing produces risk-seeking behavior; high-probability-moderate-loss framing produces risk aversion. Styx's tier structure must account for these different psychological regimes rather than treating all stake levels as equivalent.

## Mental Accounting and the Vault Metaphor

Thaler's (1999) mental accounting framework explains why financial stakes in Styx feel psychologically "real" even though money is fungible. When a user commits funds to a Styx vault, they create a new mental account. The staked money is psychologically segregated from the user's bank balance. This segregation has two consequences.

First, forfeiture triggers the pain of closing a mental account, which produces disproportionate distress relative to the nominal amount lost. The $50 in a Styx vault is experienced differently from $50 in a checking account, even though they are economically identical.

Second, the endowed-progress onboarding bonus ($5 credited at account creation) exploits the endowment effect (Kahneman, 2011). Once the user "owns" the $5 bonus within their mental Styx account, loss aversion applies to it. The bonus is not merely an incentive to sign up; it is a psychological anchor that makes the user loss-averse about their Styx balance from the very first interaction.

## Present Bias and the Demand for Commitment

The beta-delta quasi-hyperbolic discounting model (Laibson, 1997) provides the formal justification for why Styx users exist at all. The model posits a utility function U = u_0 + beta * sum(delta^t * u_t), where beta < 1 captures present bias -- the systematic overweighting of immediate gratification relative to future outcomes. When beta < 1, agents recognize (or fail to recognize) that their future selves will not follow through on current plans.

This creates endogenous demand for commitment devices. A sophisticated agent (one who knows their beta is low) will voluntarily constrain their future choices to align with their current preferences. Styx is precisely this constraint: the user's present self locks in a financial consequence that their future self cannot escape.

O'Donoghue and Rabin (1999) distinguish naive from sophisticated present-biased agents. Naifs overestimate their future self-control and are prone to over-staking (committing more than they can realistically deliver). Sophisticates calibrate accurately but may over-commit as a compensatory strategy. This distinction directly informs the Aegis safety system: velocity caps, maximum stake limits, and the 7-day cool-off period after failure all protect naive users from the consequences of their own miscalibration.

The Save More Tomorrow program (Benartzi & Thaler, 2004) demonstrated that pre-commitment to future behavior changes achieves 78% opt-in rates and sustained improvement. The key mechanism is that commitment is made in a "cold" cognitive state about behavior that will occur in a "hot" state. Styx replicates this architecture: contract creation happens during deliberation (cold), while daily compliance happens during temptation (hot). The financial consequence bridges the gap.

## Commitment Device Design Parameters

Bryan, Karlan, and Nelson (2010) provide the central academic taxonomy for commitment devices, distinguishing soft (psychological) from hard (financial/physical) devices. Their evidence shows that 28% of Filipino bank customers voluntarily chose commitment savings accounts -- demonstrating revealed demand for self-binding. Financial commitment devices consistently show stronger effects than purely psychological ones.

Four design parameters emerge from the literature as critical:

1. **Commitment stringency**: How difficult is it to escape the commitment? Styx contracts are irrevocable once the grace period expires -- maximum stringency.
2. **Stakes**: How much is at risk? Gneezy and Rustichini (2000) showed that small financial incentives ($0.10) actually *reduced* performance relative to no incentive, by crowding out intrinsic motivation. Stakes must be meaningful. This validates tiered staking with minimums above the crowding-out threshold.
3. **Monitoring**: Who verifies compliance? The Fury peer-audit network provides decentralized human monitoring, supplemented by automated sensor verification.
4. **Social visibility**: Is the commitment public? The Tavern social feature provides optional visibility, exploiting the additional motivational force of reputation and social accountability.

## Myopic Loss Aversion and Check-in Cadence

Benartzi and Thaler (1995) demonstrated that the combination of loss aversion with frequent evaluation produces myopic loss aversion: more frequent evaluation amplifies experienced loss. Styx's daily check-in cadence is a deliberate application of this principle. By requiring users to confront their stake status every day, the system maintains continuous loss-averse pressure rather than allowing the commitment to fade from salience.

This is a double-edged design choice. Excessive evaluation frequency can produce stress and anxiety beyond therapeutic benefit. The Aegis velocity caps partially mitigate this risk by preventing users from escalating stakes during periods of high emotional arousal.

## References

- Ariely, D. (2008). *Predictably Irrational*. HarperCollins.
- Benartzi, S., & Thaler, R. H. (1995). Myopic loss aversion and the equity premium puzzle. *Quarterly Journal of Economics*, 110(1), 73-92.
- Benartzi, S., & Thaler, R. H. (2004). Save More Tomorrow. *Journal of Political Economy*, 112(S1), S164-S187.
- Bryan, G., Karlan, D., & Nelson, S. (2010). Commitment devices. *Annual Review of Economics*, 2, 671-698.
- Gneezy, U., & Rustichini, A. (2000). Pay enough or don't pay at all. *Quarterly Journal of Economics*, 115(3), 791-810.
- Kahneman, D. (2011). *Thinking, Fast and Slow*. Farrar, Straus and Giroux.
- Kahneman, D., & Tversky, A. (1979). Prospect theory: An analysis of decision under risk. *Econometrica*, 47(2), 263-291.
- Laibson, D. (1997). Golden eggs and hyperbolic discounting. *Quarterly Journal of Economics*, 112(2), 443-478.
- Loewenstein, G., & Prelec, D. (1992). Anomalies in intertemporal choice. *Quarterly Journal of Economics*, 107(2), 573-597.
- O'Donoghue, T., & Rabin, M. (1999). Doing it now or later. *American Economic Review*, 89(1), 103-124.
- Thaler, R. H. (1999). Mental accounting matters. *Journal of Behavioral Decision Making*, 12(3), 183-206.
- Thaler, R. H., & Sunstein, C. R. (2008). *Nudge*. Yale University Press.
- Tversky, A., & Kahneman, D. (1992). Advances in prospect theory: Cumulative representation of uncertainty. *Journal of Risk and Uncertainty*, 5(4), 297-323.
