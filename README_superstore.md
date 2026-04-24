# 🛒 Superstore Sales Analysis

> Analyse des ventes d'une chaîne retail américaine | Python · Power BI  
> Projet portfolio — Data Analyst

---

## 📌 Contexte & problématique

**Superstore** est une chaîne de distribution américaine vendant des produits dans 3 catégories (Furniture, Office Supplies, Technology) sur 4 régions et 4 ans (2014–2017).

En tant que Data Analyst, j'ai répondu à 3 questions business :

1. **Quels produits et catégories sont réellement rentables** — et lesquels détruisent de la marge ?
2. **Les remises accordées sont-elles une stratégie gagnante** — ou génèrent-elles des pertes ?
3. **Quelles régions et segments clients faut-il prioriser** pour maximiser la rentabilité ?

---

## 🗂️ Structure du projet

```
superstore-sales-analysis/
├── README.md
├── notebook/
│   └── superstore_preprocessing.py     ← script Python complet annoté
├── exports/
│   └── superstore_clean.csv            ← dataset nettoyé, prêt Power BI
└── screenshots/
    ├── 01_vue_executive.png
    ├── 02_analyse_remises.png
    ├── 03_geographie.png
    ├── 04_segments.png
    └── 05_logistique.png
```

---

## ⚙️ Phase 1 — Preprocessing Python

**Fichier** : `notebook/superstore_preprocessing.py`  
**Environnement** : Google Colab  
**Source** : [Kaggle — Sample Superstore Dataset](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

### Ce que fait le script

| Section | Contenu |
|---|---|
| 1 — Chargement | Import du CSV, aperçu dimensions et types |
| 2 — Audit qualité | NaN par colonne, doublons, cohérence dates et métriques |
| 3 — Outliers | Détection IQR + Z-score, décisions documentées |
| 4 — Nettoyage | Suppression doublons, lignes incohérentes, standardisation strings |
| 5 — Feature engineering | 16 colonnes calculées (temporelles, financières, flags) |
| 6 — Rapport qualité | Bilan avant/après, visualisations diagnostiques |
| 7 — Export Power BI | CSV encodé utf-8-sig, dates ISO, booléens 0/1 |

### Colonnes créées (feature engineering)

```python
# Temporelles
Year, Month, Quarter, YearMonth, YearQuarter, Delivery_Days, Delivery_Speed

# Financières
Profit_Margin, Revenue_Per_Unit, Profit_Per_Unit, Estimated_Cost

# Segmentation
Discount_Band          # "Aucune", "Faible", "Modérée", "Forte"
Discount_Band_Order    # 1 à 4 pour tri correct dans Power BI

# Flags (0/1)
Is_Profitable, Is_Extreme_Loss, Is_High_Value
```

### Décisions de traitement des outliers

| Variable | Décision | Justification |
|---|---|---|
| Sales élevés | Conservés | Commandes B2B légitimes |
| Profit < 0 | Conservés | Signal business clé — lié aux remises |
| Pertes extrêmes | Conservés + flaggés (`Is_Extreme_Loss`) | Isolables dans Power BI sans suppression |

---

## 📊 Phase 2 — Rapport Power BI

**Modèle** : étoile — 1 table de faits (`fact_orders`) + table calendrier DAX (`Calendar`)  
**Pages** : 5 pages thématiques + time intelligence

### Page 1 — Vue exécutive
KPIs globaux (CA, Profit, Marge, Nb commandes), tendance mensuelle CA vs Profit sur 4 ans, répartition par catégorie.

![Vue exécutive](screenshots/01_vue_executive.png)

---

### Page 2 — Analyse des remises ⭐
**Insight principal** : les remises > 20% génèrent une marge systématiquement négative.

Le nuage de points (1 point = 1 commande) visualise la corrélation entre taux de remise et marge — la rupture à 20% est clairement visible.

![Analyse remises](screenshots/02_analyse_remises.png)

---

### Page 3 — Performance géographique
Carte choroplèthe par État, matrice région × catégorie avec mise en forme conditionnelle, classement des régions par marge.

![Géographie](screenshots/03_geographie.png)

---

### Page 4 — Segments & clients
Comparaison Consumer / Corporate / Home Office sur CA, marge et panier moyen. Top 10 clients avec mise en forme conditionnelle sur la rentabilité.

![Segments](screenshots/04_segments.png)

---

### Page 5 — Délais & logistique
Distribution des délais de livraison, performance par Ship Mode, matrice région × mode, évolution temporelle.

![Logistique](screenshots/05_logistique.png)

---

## 💡 Insights clés

**1. Les remises > 20% détruisent la marge**  
Les commandes avec remise > 40% génèrent en moyenne une perte par commande. La catégorie Furniture est la plus exposée.

**2. Tables et Bookcases sont structurellement déficitaires**  
Ces deux sous-catégories affichent un profit négatif sur l'ensemble de la période, indépendamment de la région ou du segment.

**3. La région West surperforme**  
Marge nettement supérieure à la moyenne nationale, portée par Technology et Office Supplies.

**4. Le segment Corporate a le meilleur potentiel**  
Panier moyen supérieur et marge plus élevée que Consumer — à prioriser dans la stratégie commerciale.

---

*Projet réalisé dans le cadre d'un portfolio Data Analyst — 2024/2025*
