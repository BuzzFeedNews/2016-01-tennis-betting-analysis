[![Binder](http://mybinder.org/badge.svg)](http://mybinder.org/repo/BuzzFeedNews/2016-01-tennis-betting-analysis)

# Methodology and Code: Detecting Match-Fixing Patterns In Tennis

A closer look at the data analysis behind BuzzFeed News’ investigation into corruption in tennis. 

## General Notes

**In “[The Tennis Racket](http://www.buzzfeed.com/heidiblake/the-tennis-racket),” a yearlong investigation into match-fixing in professional tennis, BuzzFeed News published findings from an original data analysis we performed. That analysis revealed many examples of one particularly suspicious pattern: heavy betting against a player, followed by that player’s loss.**

Betting patterns alone **aren’t proof of fixing**. Players can underperform for all sorts of reasons — injury, fatigue, bad luck — and sometimes that underperformance will just happen to coincide with heavy betting against them. But it's extremely unlikely for a player to underperform repeatedly in matches on which people just happen to be betting massive sums against him.

In developing this analysis, BuzzFeed News consulted with Abraham Wyner, a professor of statistics at the University of Pennsylvania, and Thomas Severini, a professor of statistics at Northwestern University.

To see the code that we used for the analysis, [go here](./notebooks/tennis-analysis.ipynb).

**An important note:** The analysis was undertaken with only the betting information that is publicly available. Tennis authorities and betting houses have access to much finer-grained data, such as the accounts placing bets, as well as forensic evidence such as phone data and bank records. Without access to such information, it is impossible to know with a sufficient degree of certainty whether these suspicious patterns are indeed the result of match fixing. For this reason, BuzzFeed News has decided not to name the players.

## Methodology

1. **Data Acquisition.** The analysis began by collecting the opening and closing odds of more than 26,000 tennis matches that occurred between 2009 and mid-September 2015. We downloaded the odds for Association of Tennis Professionals (ATP) and Grand Slam matches from seven large, independent bookmakers whose odds are available on OddsPortal.com. 

2. **Data Preparation.** BuzzFeed News prepared a dataset that contained one row for each bookmaker for each match. We then used the odds to calculate the implied chances that each player would win. The calculation is straightforward — opponent odds / (opponent odds + player odds) — and accounts for the house's cut. 

3. **Match Selection.** We excluded opening odds that implied probabilities more than 10 percentage points higher or lower than the median of all bookmakers’ opening odds for the match. (Otherwise the return of these odds toward the consensus could be mistaken for a sign of suspicious betting.) BuzzFeed News also excluded matches that were noted as “canceled” — typically a result of pre-match withdrawals — or “walkover” on OddsPortal. After removing around 500 matches based on the criteria above, **25,993 matches** remained. 

4. **Odds-Movement Calculation.** To calculate the “odds movement” for a bookmaker in a given match, BuzzFeed News looked at the difference between each player’s chance of winning (see above) implied by the opening and final odds. For example, if the opening odds suggested Player A had a 65% chance of winning, but the final odds suggested a 50% chance of winning, the “odds movement” is 15 percentage points.

5. **Player Selection.** BuzzFeed News then selected only matches where, in at least one book, the odds moved more than 10 percentage points. (This phenomenon occurred in about 11% of all matches.) We selected the 10-percentage-point cutoff based on discussions with sports-betting investigators, who said that movement above this threshold was what prompted them to give greater scrutiny to a match. We then selected players who had lost more than 10 such “high-movement” matches. **Thirty-nine players** met this criterion.

6. **Simulation.** To estimate the unlikelihood of each player’s outcomes, BuzzFeed News ran a series of simulations. Each simulation used the player’s implied chance of winning — based on each match’s opening odds — to generate a set of outcomes for each string of matches. BuzzFeed News ran the simulation 1 million times per player. The result: The estimated chance that the player would have lost as many (or more) high-movement matches as the player did, if the chances implied by the opening odds were correct. 

7. **Significance Check.** BuzzFeed News then tested each player’s results for statistical significance. Because 39 players were tested — and the more players you test, the more likely you are to encounter false positives — BuzzFeed News applied a Bonferroni correction to the results. Four players’ simulation results achieved Bonferroni significance at the 95% confidence level. For another 11 players, the results were not significant at the Bonferroni level, but would still have been expected to occur less than 5% of the time. For the full results, please see the table in the [analysis notebook](./notebooks/tennis-analysis.ipynb).
