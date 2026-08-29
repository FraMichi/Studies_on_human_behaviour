# Hot Hand, Real or Believed?

**A behavioural analysis of setting decisions in professional volleyball**

Course project — *Studies on Human Behaviour*, Data Science  
Francesca Michieletto (ID 255371)

---

## Overview

The *hot hand* is the belief that a player who has just succeeded is more likely to succeed again. This project separates two related questions: whether the effect is **real** (does previous success predict future success?) and whether it is **believed** (do decision-makers act as if it were true?).

Volleyball is well suited to this distinction because the setter chooses, before almost every attack, which teammate receives the ball. This provides a repeated and observable decision that can be reconstructed from scouting data.

The analysis uses one full season of Data Volley scouting files from two Italian Serie A2 women's teams, **Valsabbina Millenium Brescia** and **CDA Volley Talmassons FVG**, both promoted to Serie A1 at the end of the 2025/26 season.

## Research questions

| | Question |
|---|---|
| **RQ1** | Does an attacker's probability of success depend on the outcome of her previous attack? |
| **RQ2** | Is a successful attacker more likely to be selected again, after accounting for player, match, and rotation? |
| **RQ3** | Does the critical phase of a set affect attack errors or the attacking situation the team can create? |

## Main findings

### RQ1 — No significant hot-hand effect

Raw success rates are higher after a previous success (Brescia: 40.2% vs. 36.8%; Talmassons: 42.1% vs. 40.4%). However, after accounting for differences between players and matches, the effect is not statistically significant for either team (Brescia: β = +0.085, SE = 0.064; Talmassons: β = +0.031, SE = 0.071).

The differences observed in the raw rates may therefore be partly explained by player heterogeneity rather than by a dependence on the previous attack.

### RQ2 — The interpretation changes after controlling for rotation

A team rotates when it wins a rally while receiving, meaning that previous success and the attacking options available on the next action can be related through the rules of the game.

Without controlling for rotation, Talmassons shows a significant negative pattern, while Brescia shows no significant effect. After adding rotation, the Talmassons effect is no longer significant (β = −0.283*** → −0.142, n.s.), while a significant positive effect emerges for Brescia (β = +0.043, n.s. → +0.246***).

The final result is therefore consistent with a hot-hand allocation pattern for Brescia, even though RQ1 provides no evidence of a corresponding performance effect.

### RQ3 — No evidence of worse attacking performance under pressure

Talmassons shows no significant change between normal and critical phases. Brescia shows fewer attack errors (β = −0.358**) and lower high-ball use (β = −0.190**) during critical phases.

High-ball use should not simply be interpreted as a measure of tactical caution: among side-out attacks, the high-ball rate is 0.0% after a perfect reception and 59.8% after a poor reception. This shows a strong relationship between reception quality and high-ball use. The lower high-ball probability for Brescia is therefore consistent with the team reaching the attack in better conditions during critical phases, although this mechanism is not directly established by the current models.

## Method

Logistic mixed-effects models with crossed random intercepts for player and match were fitted separately for each team:

`logit P(outcome) = β₀ + β₁ · predictor + u_player + u_match`

The two grouping factors are crossed rather than nested because the same player appears across multiple matches and each match contains several players. The random intercepts account for baseline differences between players and matches.

The models were estimated using an iterative weighted least-squares procedure implemented in NumPy and tested on simulated data with known parameters before being applied to the volleyball data.

## Repository contents

~~~text
├── Hot_Hand_Volleyball_Analysis.ipynb   Full analysis pipeline (Colab)
├── Short_Research_Paper.pdf             Results-focused paper
├── Scientific_Essay.pdf                 Complete paper
├── Project_Definition.pdf               Deck 1 — project proposal
├── Project_Progress.pdf                 Deck 2 — project progress
├── Final_Presentation.pdf               Deck 3 — final results
├── dataset/
│   ├── all/                             63 .dvw files
│   ├── brescia/                         36 .dvw files (Brescia)
│   ├── talmassons/                      29 .dvw files (Talmassons)
└── README.md
~~~

The two head-to-head matches appear in both team folders. The analysis reads `all/`.

## Data

The dataset contains 63 Data Volley `.dvw` scouting files from the 2025/26 season: 36 with Brescia as the focal team and 27 with Talmassons. The two head-to-head matches are stored under Brescia, meaning that Talmassons appears in 29 matches overall.

The files cover the regular season, Coppa Italia, and promotion play-offs. Parsing produced 93,884 scouted actions, of which 8,312 are attacks performed by 23 players from the two focal teams.

The files were provided by the technical staff of Valsabbina Millenium Brescia and are **not redistributed in this repository**. The notebook reads them from a Google Drive folder, with the path specified in Section 2.

## Running the notebook

Open `Hot_Hand_Volleyball_Analysis.ipynb` in Google Colab and run all cells in order.

**Note on the setup:** Section 3 applies compatibility fixes. The current Colab environment uses recent versions of NumPy and pandas that are not fully compatible with the public version of `pydatavolley`. The fixes are applied before parsing and do not change the library's parsing logic.

## Limitations

- **Rotation:** rotation is controlled as a binary marker. A finer specification could include the attacker's actual rotational position and distinguish between front-row and back-row situations.
- **Definition of success:** success is defined as a direct point (`#`). A robustness check could also include positive attacks (`+`).
- **Longer attack sequences:** RQ1 considers only the immediately preceding attack. Future analyses could examine longer sequences to test whether hot-hand patterns emerge over multiple attempts.
- **Model validation:** the mixed-effects estimator could be compared with an established implementation as an additional validation check.
- **Player-specific effects of pressure:** RQ3 estimates an overall effect of critical phases. Future analyses could test whether this effect differs across players.
- **Build-up quality:** reception quality is strongly related to high-ball use. Future analyses could model reception and dig quality directly to investigate whether build-up quality helps explain the relationship between competitive pressure and attacking situation.

## References

- Data Project S.r.l. *Data Volley* [scouting software].
- Gilovich, T., Vallone, R., & Tversky, A. (1985). The hot hand in basketball: On the misperception of random sequences. *Cognitive Psychology*, 17(3), 295–314.
- Ittlinger, S., Lang, S., Schubert, A., & Raab, M. (2025). How cognitive biases affect winning probability perception in beach volleyball experts. *Scientific Reports*, 15, 32408.
- Köppen, J., & Raab, M. (2012). The hot and cold hand in volleyball: Individual expertise differences in a video-based playmaker decision test. *The Sport Psychologist*, 26(2), 167–185.
- MacMahon, C., Köppen, J., & Raab, M. (2014). The hot hand belief and framing effects. *Research Quarterly for Exercise and Sport*, 85(3), 341–350.
- OpenVolley. [pydatavolley](https://github.com/openvolley/pydatavolley) · [Data Volley file format manual](https://openvolley.r-universe.dev/datavolley/doc/manual.html)
- Raab, M., Gula, B., & Gigerenzer, G. (2012). The hot hand exists in volleyball and is used for allocation decisions. *Journal of Experimental Psychology: Applied*, 18(1), 81–94.

## Acknowledgements

Thanks to the technical staff of Valsabbina Millenium Brescia for providing the scouting files and for explaining the coding conventions used during data collection.
