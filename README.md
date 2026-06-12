# OTT Viewer Drop-off & Retention — Case Study
 
A data-driven investigation into why viewers abandon Season 1 of original OTT series, what content and behavioral signals predict it, and what the platform can do about it.
 
**Dataset:** 33,171 episodes across 489 shows, 16 genres, and 28 platforms.
**Overall Season 1 drop-off rate:** 14.5%
 
---
 
## 1. Where Do Viewers Leave?
 
Using the continuation rate formula `CR(k→k+1) = viewers at ep k+1 / viewers at ep k`, only a small fraction of viewers who start Episode 1 reach the end of the season. Drop-off is **not concentrated at the pilot** — Episode 1 actually has the *lowest* drop-off (3.7%) thanks to a "launch curiosity effect." The real risk builds from Episode 2 onward.
 ![image](https://github.com/garvitkumbhat619/OTT_Retention_Case_study/blob/main/EDA%20and%20analysis/VIZ2_season_arc_dropoff.png)
 
---
 
## 2. The Three Strongest Predictors of Drop-off
 
Statistical testing (point-biserial correlation, Cramér's V, Mann-Whitney U, Kruskal-Wallis) ranked every variable in the dataset:
 
| Driver | Correlation with drop-off | Verdict |
|---|---|---|
| **Cognitive Load** | r = +0.57 | Strongest single predictor |
| **Pacing Score** | r = −0.41 | pacing=8 → 0% drop-off; pacing=3 → 47.8% |
| **Hook Strength** | r = −0.30 | 7–23× higher risk at low hook, at *every* episode position |
 
Episode duration and raw episode position were **not** significant predictors (p > 0.15) — drop-off is driven by *content design*, not runtime or where an episode falls in the season.
 
![image](https://github.com/garvitkumbhat619/OTT_Retention_Case_study/blob/main/RCA/B1_feature_ranking.png)
 
---
 
## 3. Pacing — A Single Bad Sequence Can Wreck an Episode
 
A Slow episode (pacing ≤ 4) followed by another Slow episode produces ~42.7% drop-off — statistically the same as a Fast→Slow "whiplash" transition (~42.9%). But a Slow episode followed by a Fast one drops to just **2.5%**. The single worst cell in the entire dataset: **Episode 8 with pacing=3 → 71% drop-off**.
 
![image](https://github.com/garvitkumbhat619/OTT_Retention_Case_study/blob/main/RCA/B3_structural_vs_content.png)
 
---
 
## 4. The Skip-Intro Signal
 
Episodes where viewers skip the intro show **23.3% drop-off vs. 5.9%** for those who don't — a 4× risk multiplier and one of the strongest behavioral early-warning signals in the dataset.
 
![image](https://github.com/garvitkumbhat619/OTT_Retention_Case_study/blob/main/EDA%20and%20analysis/VIZ6_skip_intro_analysis.png)
 
---
 
## 5. Four Episode Archetypes (Segmentation)
 
K-Means clustering (k=4, validated via Elbow, Silhouette, Calinski-Harabász, and Davies-Bouldin) on content-design features identified four distinct episode profiles:
 
| Segment | Drop-off | Avg Watch % | Profile |
|---|---|---|---|
| 🔴 **Burnout Zone** | ~40% | ~43% | Slow pacing + weak hook + high cognitive load + dense dialogue |
| 🟡 **Prestige Tension** | ~12–18% | ~53% | High cognitive load, but rescued by a strong hook |
| 🟢 **Propulsive Gold** | 0% | ~71% | Strong hook + good pacing + accessible — the ideal blueprint |
| 🔵 **Comfort Cruise** | 0% | ~59% | Fast, light content — safe but under-engaging |
 
Content design (pacing/hook/cognitive load combination) — not genre or episode position — is the dominant driver of which segment an episode falls into.
 
![image](https://github.com/garvitkumbhat619/OTT_Retention_Case_study/blob/main/segmentation/C6_radar_overlay.png)
 
---
 
## 6. Genre Root Cause: Why Drama Underperforms
 
Drama has the platform's highest drop-off (24.2% vs. 14.5% baseline) — explained almost entirely by its content DNA: **66.3%** of Drama episodes carry high cognitive load and **49.8%** are slow-paced. Comedy, by contrast, has 0% slow-paced episodes and only 2.9% drop-off — proving these attributes are a *design choice*, not a genre inevitability.
 
![image](https://github.com/garvitkumbhat619/OTT_Retention_Case_study/blob/main/RCA/B7_genre_rca.png)

---
 
## 7. Recommendations (Prioritized)
 
| Quadrant | Recommendation |
|---|---|
| **Quick Win** | Smarter autoplay / next-episode UX for episodes flagged high-risk |
| **Quick Win** | Episode-8 recap & re-entry nudges (targets the 71% drop-off cell) |
| **Quick Win** | Mandatory pacing-recovery sequencing — never place two consecutive slow episodes |
| **Fill-in** | Pause-triggered "Interrupted Viewer" recap prompt (pause_count ≥ 6 → 39.7% drop-off) |
| **Fill-in** | Hide "Skip Intro" on Episode 1 for first-time viewers (preserve manual scrubbing) |
| **Strategic Bet** | Script guidelines: hook_strength ≥ 6, and high cognitive load permitted only with hook ≥ 7 |
| **Strategic Bet** | Gradual genre diversification for Drama-heavy platforms |
 
Each recommendation is validated against a primary KPI (e.g., episode-specific continuation rate or drop-off rate) with an accompanying A/B testing framework using user-level randomization, power-analysis-based sample sizing, and guardrail metrics.
 
---
 
## 8. Confirmed Data Anomalies (Excluded from Modeling)
 
- `retention_risk` is a **perfect proxy** for `drop_off` (100% correlation) — derived label, not an independent signal.
- `night_watch_safe = 1` → 0% drop-off across all 527 such episodes — synthetic separator, excluded to prevent leakage.
- `cognitive_load = 8` is entirely absent from the dataset — flagged as a data generation artifact.
---
 
## Key Takeaway
 
Drop-off in this dataset is **predictable and controllable**. The strongest levers — cognitive load, pacing sequencing, and hook strength — are all content-design choices made before an episode airs, not fixed properties of genre or runtime. A handful of targeted, low-cost interventions (sequencing rules, hook thresholds, and risk-aware UX) directly address the segments and episode positions responsible for the bulk of Season 1 drop-off.
