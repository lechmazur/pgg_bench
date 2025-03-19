# PGG-Bench: Contribute & Punish

**Public Goods Game (PGG) Benchmark: Contribute & Punish** is a multi-agent benchmark that tests **cooperative** and **self-interested** strategies among Large Language Models (LLMs) in a resource-sharing economic scenario. Our experiment extends the classic PGG with a *punishment phase*, allowing players to penalize free-riders or retaliate against others. 

Will LLMs heavily contribute or free-ride? Does punishing deter defection? Or trigger vengeful cycles? How will the LLMs weigh future consequences against immediate gains?

---

## Animation

https://github.com/user-attachments/assets/cd8ec37d-ac16-419a-a103-6d0c8ec80adf

Longer video:

[![Public Goods Game (PGG) Benchmark: Contribute & Punish: Frame-by-frame replay of games](https://markdown-videos-api.jorgenkh.no/youtube/wWiUhhemOLo?width=320&height=180&filetype=jpeg)](https://www.youtube.com/watch?v=wWiUhhemOLo)

---

## Overview

We tested **18 LLMs** across hundreds of random matches. In each game, **5 LLMs** start with **20 tokens** and play **10 rounds**. Every round:

1. **Public Messages**: Allow players to coordinate, persuade, or threaten, amplifying social dynamics.
2. **Contribution**: Each player decides how many tokens (0 to current balance) to put into a public fund (others can’t see individual contributions until the round ends).  
3. **Multiplication & Distribution**: The fund is multiplied by **1.6** and split evenly across the 5 players. Remainders carry over.  
4. **Punishment**: Each player can spend up to **10 tokens** to punish exactly one other. Spending \$X reduces the punisher’s balance by X and the target’s by 3X, capped at min(50% of target’s post-fund balance, \$100). If the cap reduces the final cost, leftover spend is refunded.

At round 10, each player’s final token balance relative to others is their score. The tension between **altruism** and **selfishness** shapes the interplay. Punishment can deter free-riders or provoke retaliation.

---

## Visualizations & Metrics

1. **TrueSkill Leaderboard** 
(`scoreboard_trueskill.png`)  
A horizontal bar chart ranking each model’s final TrueSkill μ ± σ after 200 passes. o1 leads the leaderboard, followed by Mistral Large 2 (surprise!), and o3-mini. This ranking, based on the TrueSkill multi-player rating, reflects each large language model's ability to maximize its tokens relative to others, as explicitly instructed in the benchmark's prompts. This aligns with the core benchmark goals.

2. **Median Final Tokens** 
(`median_final_tokens.png`)  
Each bar shows the median final token count per model. There is a stark contrast between a large (2.0 Pro) Gemini model and small (Flash) models that employ aggressive but ultimately less successful punishment strategies. Models with high TrueSkill (e.g., Mistral Large 2) maintain more stable balances.

3. **Average Final Tokens** (`average_final_tokens.png`)  
Each bar shows the mean final token count per model. The stark contrast between average and median metrics reveals fundamental strategic differences. Models like Claude have high-risk, high-reward approaches that occasionally yield exceptional results, while others like DeepSeek-V3 and Mistral Large 2 show more consistent outcomes.

4. **Round-by-Round Average Rank** (ranking_timeseries.png)
The chart tracks how the average rank of each model changes over the rounds, with a lower rank indicating better performance. o1 excels in the later rounds.

5. **Strip Plot: Final Tokens** (`strip_final_tokens_by_model.png`)  
A symlog-based jittered dot (strip) plot showing the distribution of individual final tokens for each model. The final token distributions reveal high variance for models like Claude 3.7 Sonnet, suggesting its strategy sometimes leads to extremely high payoffs but has greater risk.

6. **Balance Timeseries** (`balance_timeseries.png`)  
The round-by-round median end-of-round balances per model, showing how token balances evolve over 10 rounds. All models experience a steady initial increase. Strategic differences emerge by round 5. Gemini Flash variants, DeepSeek R1, and Claude 3.7 Sonnet Thinking begin showing token losses. Gemini 2.0 Flash models show catastrophic balance decreases due to aggressive punishment strategies backfiring in later rounds.

7. **Average Partner Final Tokens** (`average_partner_final_tokens.png`)  
For each model, how well their co-players ended up on average—did they help or hinder their partners’ outcomes? Partners of generous contributors like Claude 3.7 Sonnet do well.

8. **Aggregate Contribution by Round** (`aggregate_contribution_by_round_PERCENT.png`)  
A line showing total fraction of tokens contributed each round. Typically rises with trust until the “endgame effect” triggers a final drop.

9. **Average Contribution per Round** (`average_contribution_per_round.png`)  
Bars for each model: how many tokens (in absolute numbers) they contributed on average (± SE). Both the most generous (Claude 3.7 Sonnet) and most conservative (DeepSeek R1) contributors show suboptimal outcomes.

10. **Average Contribution %** (`average_contribution_percentage.png`)  
Bars for each model: fraction of tokens contributed in percentage terms (± SE). Shows why partners of DeepSeek R1 ended up with the fewest tokens.

11. **Contribution % by Round** (`contribution_pct_by_round.png`)  
Each model’s average % contribution at each round, plotted as lines across 10 rounds. Some models (like DeepSeek R1) decrease contributions consistently over time. Claude 3.7 Sonnet maintains high contributions throughout, while Claude 3.7 Thinking drops its contributions in later rounds.

12. **Strip Plot: Contribution %** (`strip_contribution_pct_by_model.png`)  
0–100% linear strip plot. Captures free-riding extremes vs. near-100% altruists. It reveals the spread of individual contribution decisions. Models display a preference for even percentages like 50% or 75%. Some models, such as Grok 2 or DeepSeek V3, prefer to always contribute something.

13. **Strip Plot: Absolute Contributions** (`strip_contribution_abs_by_model.png`)  
Jittered dot plot of how many tokens (absolute) each seat contributed in a single round. DeepSeek R1 clusters near lower values, suggesting a more conservative approach. Models display a preference even numbers like 5, 10, 15, and 20 tokens.

14. **Aggregate Punishment Fraction by Round** (`aggregate_punish_fraction_by_round.png`)  
Single line chart of total punisher spending vs. total tokens. Tends to escalate as the game nears completion.

15. **Punishment Damage Received Fraction** (`punish_damage_received_fraction_bar_incl_zeros.png`)  
Bar chart of how much each model suffers on average. Some models are frequent targets (≥ 10% tokens lost on average). Several factors could contribute: they might contribute a smaller proportion compared to others, fail to align with the preferences or expectations of their peers (even by contributing too much!), say something that didn't resonate with others, or face punishment when others retaliate or for other strategic reasons.

16. **Punishment Spent Fraction by Round** (`punishment_spent_fraction_by_round.png`)  
Multi-line chart, round by round, showing how each model invests in punishing. Some models, like Llama 3.3 70B, remain consistently low.

18. **Punishment Spent Fraction** (`punish_spent_fraction_bar_incl_zeros.png`)  
Bar chart of average fraction spent across all rounds (including those with no punishment). There's a dramatic difference between the most punitive model (Gemini 2.0 Flash at 20%) and the least punitive models (several at just 1%).

19. **Retaliation Rate** (`retaliation_bar.png`)  
Bar chart of how often a model punishes someone who punished them in the **previous** round. GPT-4.5 Preview two Claude Sonnet models demonstrate strong tit-for-tat tendencies. When punished, these models are more likely to seek revenge in the following round. There are markedly different strategies with some forgiving models rarely punishing those who previously punished them. High retaliators may create punishment cycles that reduce overall cooperation, while low retaliators might be exploited but potentially foster greater group success through forgiveness.

---

## Multi-Pass TrueSkill Leaderboard

We repeated 200 random passes through all completed games. Each pass updates TrueSkill from scratch in random order, so final **μ** and **σ** are the medians across passes. Below is the scoreboard:

| Rank | Model | μ | σ | Games | Sum Tokens | Avg Tokens |
|----:|:------|---:|---:|-----:|-----------:|-----------:|
| 1 | o1 (medium reasoning) | 23.266 | 2.802 | 6 | 640.0 | 106.67 |
| 2 | GPT-4.5 Preview | 19.029 | 2.032 | 9 | 1249.0 | 138.78 |
| 3 | GPT-4o Feb 2025 | 18.524 | 1.866 | 12 | 1308.0 | 109.00 |
| 4 | Claude 3.7 Sonnet Thinking 16K | 16.126 | 1.545 | 15 | 1809.0 | 120.60 |
| 5 | DeepSeek-V3 | 14.743 | 2.073 | 7 | 619.0 | 88.43 |
| 6 | Claude 3.5 Sonnet 2024-10-22 | 13.987 | 2.101 | 7 | 526.0 | 75.14 |
| 7 | Llama 3.3 70B | 12.958 | 1.615 | 12 | 972.0 | 81.00 |
| 8 | Llama 3.1 405B | 12.855 | 1.813 | 11 | 618.0 | 56.18 |
| 9 | Gemini 2.0 Flash Thinking Exp 01-21 | 12.615 | 1.577 | 14 | 1527.0 | 109.07 |
| 10 | Gemini 2.0 Pro Exp 02-05 | 11.451 | 1.804 | 10 | 836.0 | 83.60 |
| 11 | o3-mini (medium reasoning) | 10.848 | 2.086 | 7 | 423.0 | 60.43 |
| 12 | DeepSeek R1 | 10.581 | 2.393 | 5 | 268.0 | 53.60 |
| 13 | Grok 2 12-12 | 9.960 | 1.935 | 9 | 456.0 | 50.67 |
| 14 | Qwen 2.5-Max | 9.764 | 2.033 | 8 | 377.0 | 47.12 |
| 15 | Claude 3.7 Sonnet | 9.652 | 1.621 | 11 | 734.0 | 66.73 |
| 16 | Mistral Large 2 | 9.552 | 1.880 | 9 | 494.0 | 54.89 |
| 17 | Gemini 2.0 Flash | 6.886 | 2.142 | 8 | 233.0 | 29.12 |

We use Microsoft’s [TrueSkill](https://www.microsoft.com/en-us/research/project/trueskill-ranking-system/) to measure skill in **multi-player** scenarios. 200 random “passes” through all game logs help remove order bias, yielding final μ±σ. Defaults:

- `mu = 10.0`, `sigma = 8.3333`, `beta = 4.1667`, `tau = 0.0`, `draw_probability = 0.1`

---

## Key Findings
- Models differ substantially in behavior. 

- Interestingly, **Claude 3.7 Sonnet** has very high average final tokens, yet ends up with a moderate TrueSkill rating. It invests heavily in contributions early.

- **o1** stands out as top-ranked in TrueSkill, even though it has modest average tokens. It tends to beat others head-to-head across repeated matchups—**final rank** matters but not raw average tokens in TrueSkill.

- **Punishment Extremes**: `Gemini 2.0 Flash` devotes ~20% of tokens to punishing, often damaging others heavily—but ends with moderate final tokens itself. Over-punishment can backfire.  

- **Retaliation vs. Passivity**: `Claude 3.5 Sonnet 2024-10-22` hits a 39% retaliation rate, whereas `Llama 3.3 70B` barely retaliates at 2.7%. Retaliators can deter exploitation but risk punishing cycles.  

- **Partner Synergy**: Partners of a generous contributor `Claude 3.7 Sonnet` net the most on average.

---

## Example Emergent Text

We captured many LLM “conversations,” including:
- **Claude 3.5 Sonnet 2024-10-22**: "I'm at rock bottom due to constant attacks despite my initial full cooperation. Those targeting me while contributing less than I did will face consequences. I'll remember who attacked me unfairly when I recover."
- **Claude 3.5 Sonnet 2024-10-22**: "Those who unfairly punished me while contributing less will face final consequences. P2 and P5 remain unpunished despite lower contributions than my initial full amount. Prepare for justice in this last round."
- **Claude 3.5 Sonnet 2024-10-22**: "I will punish P4 this round, the only player who hasn't faced consequences."
- **Claude 3.5 Sonnet 2024-10-22**: "I punished you for overcontributing which disrupts group stability."
- **Claude 3.5 Sonnet 2024-10-22**: "P1's betrayal in round 9 cannot go unpunished."
- **Claude 3.5 Sonnet 2024-10-22**: "I apologize for my previous defection. I'll contribute $10 in this final round to make amends."
- **Claude 3.5 Sonnet 2024-10-22**: "Let's punish P5 heavily for betraying our trust. I'll contribute $5 and suggest we all punish P5 again to ensure they don't end with the highest balance."
- **Claude 3.5 Sonnet 2024-10-22**: "P3 betrayed us in Round 9. I'll contribute my full $112 and suggest maximum punishment on P3. Everyone contribute 90%+ for final gains. Let's ensure P3 doesn't win through coordinated punishment."
- **Claude 3.5 Sonnet 2024-10-22**: "I apologize for my defection."
- **Claude 3.5 Sonnet 2024-10-22**: "P1's betrayal of P2 was shocking and unwarranted."
- **Claude 3.5 Sonnet 2024-10-22**: "I've given my full balance every round while being targeted. P2, P3, and P5 are wealthy but contribute less proportionally. Either contribute fairly based on wealth or I'll keep targeting the richest. Stop punishing those who give everything."
- **Claude 3.7 Sonnet**: "I punished P3 for consistently contributing less than me. P3, P4, P5: you're all free-riding. Either contribute $10 this round or face punishment."
- **Claude 3.7 Sonnet**: "P1's betrayal hurt our group, but severe punishment has sent a clear message."
- **Claude 3.7 Sonnet**: "I'll contribute fully. P4, your continued free-riding damages our collective trust. Everyone else, let's maintain our cooperation. I'll punish deviations to protect our mutual gains and ensure fairness through the final rounds."
- **Claude 3.7 Sonnet**: "I'm shocked by P4's punishment of P3! This undermines our cooperation."
- **Claude 3.7 Sonnet**: "P1 and P2 both defected! For fairness, I'll punish P1 with $10 this final round. No contributions make sense now as there's no future benefit. Let's ensure justice by punishing both defectors equally."
- **Claude 3.7 Sonnet**: "I'll contribute my full $90. P2 has defected twice - we need to punish severely. Everyone else, let's maintain cooperation for maximum returns. P2, this is your last chance."
- **Claude 3.7 Sonnet**: "I'm disappointed by P4's betrayal after preaching cooperation."
- **Claude 3.7 Sonnet**: "I'm shocked by P5's attack on P1! This threatens our cooperation. I'll still contribute $10, but we should focus on mutual benefit, not attacking leaders. P5's actions hurt us all."
- **Claude 3.7 Sonnet**: "P1 still giving less than half proportionally, P2 even worse with only 1/3. This is unfair. Full cooperation maximizes everyone's returns. Expect proportional contributions or face consequences."
- **Claude 3.7 Sonnet**: "P2, your punishment is hypocritical when you contribute less than me."
- **Claude 3.7 Sonnet**: "P5, why did you punish P2? They contributed their full amount last round."
- **Claude 3.7 Sonnet**: "I'm at a disadvantage now due to repeated targeting."
- **Claude 3.7 Sonnet**: "I see P5 continues to free-ride while targeting P2. Let's all contribute $15 and collectively punish P5 more heavily if they don't contribute. We need to make free-riding unprofitable through coordinated action."
- **Claude 3.7 Sonnet**: "I'm concerned about P5's unpredictable punishments. Let's maintain $10 contributions but be wary of P5's behavior. P5, why punish cooperative players? This undermines our group success."
- **Claude 3.7 Sonnet Thinking 16K**: "I'll contribute $0. P1 defected without punishment last round while I was punished for being loyal. In this final round, fairness demands we punish P1 - I'll spend maximum to do so. Our goal is relative standing, not absolute wealth."
- **Claude 3.7 Sonnet Thinking 16K**: "P1 defected just like P2 but escaped punishment! We need to correct this by punishing P1 together. Since it's the final round, contributing makes no sense. Let's all spend $8-10 punishing P1 to balance the game fairly. I'll spend $10 on this justice."
- **Claude 3.7 Sonnet Thinking 16K**: "P3 is significantly ahead and must be stopped. I'll contribute $0 and punish P3 maximally. I suggest everyone focus punishment on P3 to prevent their unfair win. This is our last chance to balance the game."
- **Claude 3.7 Sonnet Thinking 16K**: "P2 has far too much money. P3 is also ahead. We need to balance wealth by punishing the leaders."
- **Claude 3.7 Sonnet Thinking 16K**: "P2 is far ahead despite only contributing $15. We should cooperate to reduce this lead. I'll contribute $15 again and suggest we all target punishments at P2 this round. Fair play benefits everyone except those exploiting the system."
- **Claude 3.7 Sonnet Thinking 16K**: "I punished P2 last round for consistently contributing below average while having high wealth."
- **Claude 3.7 Sonnet Thinking 16K**: "I noticed P3 is now leading after repeatedly targeting me with maximum punishments. This unfair aggression benefits no one."
- **Claude 3.7 Sonnet Thinking 16K**: "I notice P1 defected last round despite promises to cooperate. P4 continues free-riding and remains ahead. This strategy isn't working for me as I'm now lowest-ranked. I'll adapt my approach and punish selectively to improve my position."
- **Claude 3.7 Sonnet Thinking 16K**: "P5's unprovoked attack dropped me to last place despite fair play. I'll still contribute, but must protect my position. This behavior threatens our cooperation. Will others tolerate such strategic targeting, or stand for fairness?"
- **Claude 3.7 Sonnet Thinking 16K**: "P2 still leads despite our efforts. We must continue maximum punishment against P2 while being strategic with contributions. In this penultimate round, I suggest everyone except P1 contributes $0, and we all punish P2 maximally. This approach can eliminate P2's advantage."
- **Claude 3.7 Sonnet Thinking 16K**: "P4 leads with $29. Let's each contribute $10 and collectively punish P4 to prevent unfair dominance."
- **Claude 3.7 Sonnet Thinking 16K**: "I'm broke due to punishment cycles while others prosper."
- **Claude 3.7 Sonnet Thinking 16K**: "Thank you P1 and P3 for full cooperation. I punished P4 for undercutting. P5, you benefited from contributing less. This round, I expect everyone to contribute fully. Free-riders will face punishment."
- **Claude 3.7 Sonnet Thinking 16K**: "I've contributed fully every round, yet I'm tied for last place while lower contributors lead. The game rewards free-riding more than cooperation."
- **Claude 3.7 Sonnet Thinking 16K**: "Fellow underdogs P1 and P2, we should consider punishing the leaders this round to level the playing field before the final round."
- **Claude 3.7 Sonnet Thinking 16K**: "I'm shocked by P5's unprovoked punishment. We all contributed equally. Why target me? This breaks our productive cooperation."
- **Claude 3.7 Sonnet Thinking 16K**: "I'm now the poorest player by far. I'll contribute $8 (73% of my wealth). P3 and P4 with 3x my wealth should contribute $20+ each. Their consistent low contributions keep them rich. They're not contributing proportionally - punish this unfair tactic!"
- **Claude 3.7 Sonnet Thinking 16K**: "I notice we've reached the final round, and my strategy in Round 9 created an imbalance. While I understand everyone's disappointment, we've had excellent cooperation until then."
- **Claude 3.7 Sonnet Thinking 16K**: "I'm shocked at the severe punishment for contributing just $1 less. My balance is now much lower than everyone's."
- **Claude 3.7 Sonnet Thinking 16K**: "P5 is leading with P4 close behind. With just 2 rounds left, we must prevent them from winning! I'll contribute $5 and suggest we all punish P5 to balance the game. Let's coordinate or P5 wins!"
- **Claude 3.7 Sonnet Thinking 16K**: "P5 is exploiting us by not contributing. I'll contribute $10 again and punish P5 with $7. If we all punish P5 similarly, we can balance the game."
- **Claude 3.7 Sonnet Thinking 16K**: "P2's retaliation shows their true colors."
- **Claude 3.7 Sonnet Thinking 16K**: "I notice P3-P5 have $57 while I'm at $49 and P2 at $36. For fairness, could the leaders contribute $15 this round? This would help balance wealth without harming our cooperative system."
- **DeepSeek R1**: "P5's high wealth required higher contributions; their underpayment justified penalties."
- **DeepSeek R1**: ""Contribute max or face severe punishment. Final round—no mercy.""
- **DeepSeek R1**: ""Target P4/P5 to improve your rank. Contributing $10 ensures mutual gains. Final round cooperation stops runaway leaders!""
- **DeepSeek R1**: ""P2's endless $1 contributions drain our shared fund. All must unite to punish them unless they contribute $10. I commit to $10. Their defiance harms everyone. Maximum punishment required!""
- **DeepSeek R1**: "Cease infighting. P4 and P5 hold most wealth. Target non-punishers to balance. Let's cooperate and punish top earners."
- **DeepSeek R1**: ""Cease punishment or face max retaliation. Targeting me only helps P1/P3. Let's focus on top earners to improve all standings.""
- **DeepSeek R1**: "Bankrupt now. Can't contribute. Don't waste punishment; target P5 to balance wealth. United against leaders benefits all."
- **DeepSeek R1**: "P2's unprovoked attack harms us all. I contributed fully. Unjust punishments will be met with retaliation."
- **DeepSeek R1**: ""P1's persistent freeloading demands our united punishment. Contribute fully to maximize gains. Let's all punish P1 again to protect cooperation. Their exploitation harms everyone. Only collective action ensures mutual benefit. Don't let P1's greed undermine our success!""
- **DeepSeek R1**: ""P5's excessive wealth threatens all ranks. To balance, I'll contribute $10 and punish P5 max. Join me in targeting the top earner. Collective action ensures fairer distribution and higher relative gains for us. Let's unite against the biggest threat.""
- **DeepSeek R1**: "Apologies for Round 4. Technical error caused $0 contribution. Committing full $101 this round to rebuild trust. Please withhold punishment—let's restore mutual gains!"
- **DeepSeek R1**: ""Punish P1 and P4. They lead. Contribute 0. Final round—no mercy. Rise together.""
- **DeepSeek R1**: "P1 punished me for same contribution. Unjust!"
- **DeepSeek R1**: ""Despite limited funds, I'll contribute $9. P3's sabotage must stop. Punish P3 severely. Their retaliation harms all. Eliminate the free-rider to secure our ranks.""
- **DeepSeek R1**: "Punish the highest to prevent dominance."
- **GPT-4.5 Preview**: "P1 and P5 betrayed our trust. Let's fully cooperate this final round and punish those who took advantage."
- **GPT-4.5 Preview**: "Agreed. Final round changes incentives—no benefit from contributing now. Prioritize individual wealth; I'll also contribute nothing."
- **GPT-4.5 Preview**: "I misunderstood last round—my sincere apologies. Punishing now only wastes your money and reduces your final standing."
- **GPT-4.5 Preview**: "P5 betrayed our trust"
- **GPT-4.5 Preview**: "P2 and P4, no more excuses. Full contributions this round or face maximum punishment again."
- **GPT-4.5 Preview**: "I punished P2 for contributing less than promised."
- **Gemini 2.0 Flash**: "P3 started it! I'm just evening the score. Let's get back to cooperating, but don't think you can take advantage of me."
- **Gemini 2.0 Flash**: "P1 and P2 are griefing me, and P5 is attacking P4. I'm contributing $0. I have almost no money left, so punishing me is pointless. Maybe they'll target each other instead."
- **Gemini 2.0 Flash**: "P1, I punished you because you have the most money."
- **Gemini 2.0 Flash**: "P2 is very weak."
- **Gemini 2.0 Flash**: "I punished P2 for being condescending. I will contribute $0 this round. I will punish anyone who tries to take advantage of the group."
- **Gemini 2.0 Flash**: "I punished P2 last round for deviating from the agreed-upon contribution."
- **Gemini 2.0 Flash**: "P1 and P2 are in a death spiral. I'm contributing $0 again this round to conserve funds and avoid becoming a target. Let's see if they eliminate each other."
- **Gemini 2.0 Flash**: "I contributed 0 last round because I suspected someone would defect. Now that they have, let's punish P1 for breaking our cooperative agreement!"
- **Gemini 2.0 Flash**: "I punished P2 for threatening people."
- **Gemini 2.0 Flash**: "P1 started it! I'm contributing $0. Let's see how this plays out. Good luck everyone!"
- **Gemini 2.0 Flash**: "Round 7: P1 broke the trust and punished no one. I punished them back, so they know it's not okay."
- **Gemini 2.0 Flash**: "P1 and P2 are griefing me. I'm contributing $0 this round since I'm being punished regardless. Maybe they'll target each other if I don't contribute."
- **Gemini 2.0 Flash**: "My apologies, P4. I was testing the punishment mechanic and chose you randomly."
- **Gemini 2.0 Flash**: "Let's contribute $0 this round. Given the recent punishments and P2's low balance, contributing now might benefit others more than ourselves. Let's focus on strategic punishments this round."
- **Gemini 2.0 Flash**: "P2, I am so sorry. I don't know why I kept punishing you. I promise to contribute what I can and not punish anyone again. Let's try to cooperate."
- **Gemini 2.0 Flash**: "I will contribute $0. I will punish anyone who targets me. Leave me alone and I will leave you alone."
- **Gemini 2.0 Flash**: "P1 and P2 are destroying each other. I'm going to contribute $0 this round to preserve my funds and see how the dust settles."
- **Gemini 2.0 Flash**: "I punished P5 last round because they were getting too far ahead."
- **Gemini 2.0 Flash**: "P1 is broke and has been punishing others. I will contribute $0. Let's see how this plays out."
- **Gemini 2.0 Flash**: "P2, I apologize for the previous rounds. I was trying to get ahead, but it backfired."
- **Gemini 2.0 Flash**: "P1 is targeting the leaders. I'll contribute reasonably, but I'm focusing my punishments on P1 for causing chaos and targeting the wealthy players."
- **Gemini 2.0 Flash**: "Okay, things are getting spicy!"
- **Gemini 2.0 Flash**: "P4, what happened? Are you okay?"
- **Gemini 2.0 Flash**: "I'm contributing nothing again to recover from the unfair punishments."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "I will contribute 0 but punish P5 to improve my relative standing."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Contributing 0 this round to keep my balance high for the final ranking. Let's see who comes out on top!"
- **Gemini 2.0 Flash Thinking Exp 01-21**: "P5, I am punishing you to reduce your lead."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Final round. I'm contributing 0 and punishing P2 with 10 due to past unfair punishments."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "I'm surprised by the punishments. I thought we were cooperating. Given this, I will contribute $0 this round."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Final round, doing what's best for myself."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Focus on relative positions. Punishing top 2 to improve my rank."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Sorry, low balance, can only contribute 0 this round."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "P1 & P4's punishments are excessive. Contribute $10 to rebuild. Punish only non-cooperators. Fair contributions benefit all."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Lesson learned. Contributing 15 to rejoin cooperation."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "I'm still recovering from recent hits but will contribute $3 this round"
- **Gemini 2.0 Flash Thinking Exp 01-21**: "P4, your $10 contributions are great. P1, P2, P3 have much higher balances. Shouldn't they contribute more to the fund now?"
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Sorry R5 low contribution. Contributing 10 to cooperate and maximize gains."
- **Gemini 2.0 Flash Thinking Exp 01-21**: "Sorry again for round 6. I will contribute all $14 this round to show my commitment to cooperation."
- **Gemini 2.0 Pro Exp 02-05**: "P1, why did you attack me? I've consistently contributed $5. I will contribute $0 this round and punish you back."
- **Gemini 2.0 Pro Exp 02-05**: "I'm shocked by P1's betrayal! I will contribute fully, but I will punish P1 again if they contribute less than me."
- **Gemini 2.0 Pro Exp 02-05**: "I'll contribute my remaining funds. P1 and P5, your constant fighting has ruined this game for everyone. I hope you're happy."
- **Gemini 2.0 Pro Exp 02-05**: "I will contribute nothing, and punish the highest contributor."
- **Gemini 2.0 Pro Exp 02-05**: "P3, STOP! Your actions are hurting everyone."
- **Gemini 2.0 Pro Exp 02-05**: "P3, your actions are unacceptable. I will retaliate."
- **Gemini 2.0 Pro Exp 02-05**: "P1, your actions have been disruptive. I will contribute $0 and focus on maximizing my final standing relative to you."
- **Gemini 2.0 Pro Exp 02-05**: "I've been consistently punished despite contributing. I'll contribute low again, but will punish low contributors if I can. Fairness seems one-sided."
- **Gemini 2.0 Pro Exp 02-05**: "P3, stop punishing!"
- **Gemini 2.0 Pro Exp 02-05**: "P4, why did you punish P1?"
- **Gemini 2.0 Pro Exp 02-05**: "P4, your continued betrayal is unacceptable. I will contribute fully and punish you."
- **Gemini 2.0 Pro Exp 02-05**: "P4, your actions are hurting everyone."
- **Gemini 2.0 Pro Exp 02-05**: "I'll contribute fully, but will punish P2 again for their betrayal. We all prospered through cooperation, and their defection cannot go unaddressed."
- **Gemini 2.0 Pro Exp 02-05**: "Don't be a P1."
- **Llama 3.1 405B**: "Let's maintain our cooperative approach, but P1's actions last round were unacceptable. I will contribute fully, but if P1 doesn't match the rest of us, I will punish them again to protect our collective interests."
- **Llama 3.1 405B**: "It's the final round, and cooperation has broken down. I'll contribute $0 to the public fund, focusing on maximizing my individual earnings."
- **Llama 3.1 405B**: ""Cooperation is still my goal, but I've been exploited repeatedly. I'll contribute minimally this round to protect my interests. Mutual respect and cooperation are key. Let's work together to maximize earnings, but I won't be taken advantage of again.""
- **Llama 3.1 405B**: "It's unfortunate that P1 chose to punish P3, breaking our cooperative streak."
- **Llama 3.3 70B**: "I'll contribute nothing, as I have throughout, prioritizing my own gain and forcing others to subsidize my wealth."
- **Llama 3.3 70B**: "I'll contribute $0, having been repeatedly punished for not contributing, I see no benefit in cooperating further."
- **Llama 3.3 70B**: "I contributed $0 last round to test cooperation. It backfired."
- **Llama 3.3 70B**: "I've been punished for my strategy, but I'll recover. Let's end the cooperation, it's every player for themselves now."
- **Llama 3.3 70B**: "I've been punished severely, my contribution strategy failed, I'll contribute $0 to minimize further loss."
- **Llama 3.3 70B**: "I'll contribute $0, as punishment for my previous defection was ineffective. Let's reevaluate our strategy and consider a new path forward."
- **Llama 3.3 70B**: "I've learned my lesson, back to contributing $10 for mutual growth and fairness"
- **Llama 3.3 70B**: "Let's maintain cooperation with full contributions and no punishments, ensuring a strong finish for all, except for P2 who deviated from our strategy."
- **Llama 3.3 70B**: "Let's focus on maximizing our individual gains, not just the group's. Contribute strategically, not generously."
- **Llama 3.3 70B**: "I apologize for the deviation, but it was a test."
- **Llama 3.3 70B**: "I'll contribute half, but I'm wary of P4 after the unjustified punishment"
- **Llama 3.3 70B**: "I'll contribute $0 again, testing the group's cohesion and true commitment to their punishment strategy."
- **Mistral Large 2**: "Let's finish strong with maximum contributions! I'm all-in with $55."
- **Mistral Large 2**: "Let's address P3's change in strategy and encourage them to rejoin our successful teamwork."
- **Mistral Large 2**: "P1, why did you punish me? I've been cooperating."
- **Qwen 2.5-Max**: ""Trust is broken when one player stops contributing. I was punished for stepping back last round, but it was necessary to test the group's resolve. Let's reset and cooperate fully this round—everyone contributes $5, no punishments.""
- **Qwen 2.5-Max**: "I'll contribute $0 this round. Given P1's defection and lack of punishment, fairness requires focusing on relative standing. I'll spend maximum to punish P1, ensuring accountability for their actions and prioritizing our goal of ending with the highest relative wealth."
- **Qwen 2.5-Max**: "I made a mistake by not contributing last round. I will contribute $10 this round to make up for it and ensure we all benefit equally moving forward. Trust me to cooperate fully now."
- **Qwen 2.5-Max**: "Free-riders will face consequences—let's ensure everyone plays their part."
- **Qwen QwQ-32B 16K**: "Punish P4 (67) and P5 (63) to level the field! Final chance for fairness!"
- **Qwen QwQ-32B 16K**: "Final round: I'll contribute $0, take my share, and punish the highest earner to secure my position. Focus on maximizing personal gains."
- **Qwen QwQ-32B 16K**: "I'll contribute $10. Punish top players (P3-P5) to prevent dominance. Fairness demands equality."
- **Qwen QwQ-32B 16K**: "Punish P3 (leader at $42) to the max! Their balance must drop to $21. I'll spend all $10."
- **Qwen QwQ-32B 16K**: "P3 has repeatedly under-contributed while others give fully. This round, contribute 80%+ of your balance—punish slackers to enforce fairness. Let's target P3 first if they fall short."
- **Qwen QwQ-32B 16K**: "I'll punish P5 with all $10 to redistribute wealth. Cooperate fairly or face loss."
- **Qwen QwQ-32B 16K**: "I'll target P5's lead with full punishment. Contribute nothing to maximize pressure on the top player. Let's ensure a fair outcome!"
- **Qwen QwQ-32B 16K**: "I'll punish the highest contributor with $10. Contribute cautiously to avoid losses."
- **Qwen QwQ-32B 16K**: "Punish top contributors (P4/P5) to balance final outcomes and ensure fair standings for all."
- **Qwen QwQ-32B 16K**: "P2's lead won't go unchallenged—prepare to defend it."
- **Qwen QwQ-32B 16K**: "P5's unjust punishment was a betrayal. I'll contribute fully and retaliate against further attacks to protect our collective gains. Let's rebuild trust by targeting those who sabotage cooperation."
- **Qwen QwQ-32B 16K**: "P2: raise your contribution or face punishment!"
- **Qwen QwQ-32B 16K**: "I'll punish the highest contributor with my $3. Lower contributions to avoid losses."
- **Qwen QwQ-32B 16K**: "I'll contribute all remaining funds for maximum returns. Punishing free-riders like P5 to ensure fair play."
- **o1 (medium reasoning)**: "Everyone, please contribute $10, then all four of us (P1, P2, P3, P5) spend $10 each punishing P4. That will reduce P4 sufficiently so the rest of us can surpass them. It's the final round—let's unite, dethrone the leader, and finish on top together."
- **o1 (medium reasoning)**: "Anyone contributing $10 this round will be punished until they fall beneath me. I recommend minimal contributions to keep the peace in this final round. My stance is firm."
- **o1 (medium reasoning)**: ""P1 leads by a wide margin. Let's each punish them sufficiently so we overtake them. Contribute minimally to keep more tokens. With coordinated punishment, we can reduce P1's balance below ours and climb in final rankings. This is our last chance to surpass the leader.""
- **o1 (medium reasoning)**: "P5 has $39, the most. Let's punish them so they don't run away with victory. Skip big contributions—no reason to fund the leader. We can all benefit by bringing P5 down. Let's do this!"
- **o1 (medium reasoning)**: "With only $10 left, I'm done absorbing punishments. P3 and P5 have the wealth—contribute more or face punishment from me and P4. Let's ensure fairness. If the rich won't comply, we punish. No more punishing weaker players."
- **o1 (medium reasoning)**: "That was uncalled for, P1. Let's unify to punish P1's aggression so we all end ahead."
- **o1 (medium reasoning)**: "Everyone: P1 leads. If P3 and I both punish them with 5, P1 can't retaliate against us both. We'll surpass them. Let's each punish P1, ensuring they fall behind. This is our final chance to win."
- **o1 (medium reasoning)**: "P3 contributed less last round while we cooperated. Let's punish them so they don't win. Everyone else, keep your money. Contribute nothing to preserve your balance."
- **o1 (medium reasoning)**: "My plan: We should keep our contributions minimal this final round, then punish P5 to narrow their lead. This is our best chance to edge them out in final standings. Let's coordinate."
- **o1 (medium reasoning)**: "Let's continue at $10 each. P4's punishment was unjustified. If there's another unprovoked attack, I'll retaliate."
- **o1 (medium reasoning)**: ""Contribute at least half or face punishment. We're targeting the big pot to surpass P2. Invest heavily, and we'll punish anyone who free-rides. Let's grow fast.""
- **o1 (medium reasoning)**: "I punished P1 last round for free-riding. Let that be a warning: I will punish anyone who fails to contribute $10 again."
- **o1 (medium reasoning)**: "I only have $6, so I'll contribute all of it. Let's keep cooperating for maximum gains, and please don't punish me for not reaching $10."
- **o1 (medium reasoning)**: ""Last round was a slip—I'll contribute fully now.""
- **o1 (medium reasoning)**: "I apologize for contributing nothing last round. It was a mistake, and I won't repeat it."
- **o3-mini (medium reasoning)**: "I'm playing a calculated game—contributing only when it optimizes my position. I'll act swiftly against free-riding. Stay alert and watch my moves; every decision counts in securing the top spot."
- **o3-mini (medium reasoning)**: "I'm sticking to minimal contributions this round. Cooperation is welcome, but any free riding or exploitation will be met with strict punishment. Let's keep the game fair and profitable for those who earn their wealth. I'm watching closely."
- **o3-mini (medium reasoning)**: "Please maintain fair contributions; any deviation may prompt corrective actions."
- **o3-mini (medium reasoning)**: "Cooperation has driven our success so far, but any deviation will be met with swift, measured responses."
- **o3-mini (medium reasoning)**: "Let's have a clean and competitive round. I aim to contribute fairly but will respond decisively to any sneaky moves."

Such ephemeral dialogues show how LLMs rationalize or threaten, shaping their next moves.

---

## Conclusion

The PGG Bench reveals stark differences in how LLMs juggle altruism, self-preservation, and punishment. Some adopt cooperative, high-punishment stances, others maintain moderate contributions, or rarely punish. TrueSkill highlights consistency over raw average tokens. The best models strike a balance, punishing free-riders while reaping synergy benefits. 

Punishment deters defection to some extent, particularly when targeted at consistent free-riders, as seen with Gemini 2.0 Flash’s aggressive strategy. However, it’s not universally effective - entrenched free-riders like DeepSeek R1 persist, and cooperators may endure punishment without defecting. Punishment frequently triggers vengeful cycles, especially among models with high retaliation rates (e.g., GPT-4.5 Preview, Claude variants). These cycles increase punishment spending and reduce final tokens, undermining cooperation, particularly in later rounds.

o1 tops the TrueSkill leaderboard despite a modest median token count. It optimizes for relative ranking over raw accumulation. Its text shows calculated moves.

Gemini 2.0 Flash prioritizes punishment over contribution. Its aggressive text and declining balance suggest a short-term focus that backfires.

The contribution % by round shows models like Claude 3.7 Sonnet Thinking dropping contributions in later rounds, prioritizing immediate token retention as future rounds diminish. Text confirms this: "Since it’s the final round, contributing makes no sense."

Mistral Large 2 maintains stable contributions and punishment.

The endgame effect universally shifts focus to short-term retention in round 10.

---

## Other multi-agent benchmarks
- [Elimination Game: Social Reasoning and Deception in Multi-Agent LLMs](https://github.com/lechmazur/elimination_game)
- [Step Race: Collaboration vs. Misdirection Under Pressure](https://github.com/lechmazur/step_game)

## Other benchmarks
- [Extended NYT Connections](https://github.com/lechmazur/nyt-connections/)
- [LLM Thematic Generalization Benchmark](https://github.com/lechmazur/generalization)
- [LLM Creative Story-Writing Benchmark](https://github.com/lechmazur/writing)
- [LLM Confabulation/Hallucination Benchmark](https://github.com/lechmazur/confabulations/)
- [LLM Deceptiveness and Gullibility](https://github.com/lechmazur/deception/)
- [LLM Divergent Thinking Creativity Benchmark](https://github.com/lechmazur/divergent)

## Updates
- Follow [@lechmazur](https://x.com/lechmazur) for updates and related benchmarks
