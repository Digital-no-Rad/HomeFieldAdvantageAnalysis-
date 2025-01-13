# HomeFieldAdvantageAnalysis-
To do Hypothesis testing to see what affect home field has over the 4 major NA sports
git clone https://github.com/radhe/HomeFieldAdvantageAnalysis.git
git add .
git commit -m "Initial commit"
git push
import pandas as pd
from scipy.stats import ttest_ind, f_oneway

# Mock dataset
data = {
    "League": ["NBA", "NBA", "NHL", "NHL", "MLB", "MLB", "NFL", "NFL"],
    "Team": ["Lakers", "Celtics", "Maple Leafs", "Oilers", "Yankees", "Dodgers", "Patriots", "Cowboys"],
    "Home Wins (%)": [75, 70, 65, 60, 58, 55, 85, 80],
    "Away Wins (%)": [40, 45, 50, 48, 47, 46, 55, 50],
}

df = pd.DataFrame(data)

# T-tests for each league
leagues = df["League"].unique()
for league in leagues:
    league_data = df[df["League"] == league]
    t_stat, p_value = ttest_ind(league_data["Home Wins (%)"], league_data["Away Wins (%)"])
    print(f"{league} - T-statistic: {t_stat:.2f}, P-value: {p_value:.3f}")

# ANOVA across leagues
anova_stat, anova_p = f_oneway(
    df[df["League"] == "NBA"]["Home Wins (%)"] - df[df["League"] == "NBA"]["Away Wins (%)"],
    df[df["League"] == "NHL"]["Home Wins (%)"] - df[df["League"] == "NHL"]["Away Wins (%)"],
    df[df["League"] == "MLB"]["Home Wins (%)"] - df[df["League"] == "MLB"]["Away Wins (%)"],
    df[df["League"] == "NFL"]["Home Wins (%)"] - df[df["League"] == "NFL"]["Away Wins (%)"],
)
print(f"ANOVA - F-statistic: {anova_stat:.2f}, P-value: {anova_p:.3f}")
