# ARGUS
### Automated Recognition and Grading of Unconfirmed Stellar candidates

> A machine learning pipeline that identifies exoplanet candidates 
> hiding in NASA's Kepler telescope data.

---

## What is this?

NASA's Kepler telescope stared at 150,000 stars for 4 years, 
measuring their brightness every 30 minutes. When a planet passes 
in front of its star, it blocks a tiny fraction of light — creating 
a signal called a transit.

Of the 9,564 stars Kepler flagged as interesting, 1,979 remain 
unconfirmed — neither proven to be real planets nor ruled out as 
false positives.

ARGUS is a machine learning model trained to tell the difference 
between real planets and false positives. Applied to those 1,979 
unconfirmed candidates, it flagged **828 as likely real planets**.

---

## Results

| Metric | Value |
|--------|-------|
| Training set | 7,585 labeled stars |
| Model accuracy | 92% |
| Unconfirmed candidates analyzed | 1,979 |
| Flagged as likely planets | 828 |
| Top candidates (confidence = 1.0) | 20 |

The model achieves 92% accuracy without using NASA's own disposition 
score — relying purely on physical measurements like planet radius, 
orbital period, and stellar properties.

---

## How it works
```
Raw NASA Kepler catalog
        ↓
Feature extraction (planet radius, period, stellar properties...)
        ↓
Random Forest classifier trained on 7,585 labeled examples
        ↓
Applied to 1,979 unconfirmed candidates
        ↓
828 flagged as likely planets
```

**Key finding:** Planet radius is the single most important feature. 
Real planets tend to be small. Large signals are usually eclipsing 
binary stars mimicking a planet transit. Multi-planet systems are 
almost always real — the model discovered this from data alone.

---

## Top 20 Candidate Planets

Stars NASA hasn't confirmed, that ARGUS predicts are real planets:

| Kepler ID | Planet Radius (Earth radii) | Orbital Period (days) | Confidence |
|-----------|----------------------------|----------------------|------------|
| 7107802 | 1.07 | 5.47 | 1.0 |
| 9896018 | 1.60 | 10.30 | 1.0 |
| 9347899 | 1.94 | 9.62 | 1.0 |
| 11253711 | 2.88 | 17.79 | 1.0 |
| 7673192 | 1.17 | 16.53 | 1.0 |
| 6871071 | 1.60 | 7.66 | 1.0 |
| 4478168 | 0.99 | 8.03 | 1.0 |
| 3765917 | 2.09 | 11.82 | 1.0 |
| 5511659 | 1.98 | 17.24 | 1.0 |
| 8949925 | 1.98 | 13.25 | 1.0 |

*Full list of 828 candidates available in results/candidates.csv*

---

## What I found interesting

- Removing NASA's confidence score (koi_score) dropped accuracy 
  from 99% to 92% — proving the model was learning real physics, 
  not just copying NASA's answer
- Stellar temperature uncertainty is the second most important 
  feature — poorly characterized stars are harder to confirm
- Multi-planet systems are significantly more likely to be real — 
  the model discovered this independently from data

---

## How to run it

1. Open `ARGUS.ipynb` in Google Colab
2. Run all cells in order
3. Results saved to `results/candidates.csv`
```python
# Quick start
!pip install lightkurve astroquery
```

---

## Stack

- Python, Google Colab
- lightkurve — NASA's official Kepler/TESS data library  
- scikit-learn — machine learning
- NASA Exoplanet Archive — training data

---

## Status

- [x] Data pipeline built
- [x] Baseline model (89% accuracy)
- [x] Improved model (92% accuracy, no score leakage)
- [x] 828 candidate planets flagged
- [ ] Raw light curve verification of top candidates
- [ ] Neural network upgrade
- [ ] Paper write-up

---

## Inspired by

Matteo Paz's 2025 discovery of 1.5 million variable objects in 
NASA NEOWISE data — proving that meaningful astronomical discoveries 
can come from a laptop and open data.

---

*Built by Anish — github.com/Hyperhypno*  
*Started April 2026*
