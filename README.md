# Network Scan Detector

A beginner-friendly Jupyter notebook that trains a small machine-learning model
to classify **fictional network connections** as either normal traffic or likely
**port scanning**.

This project was built during a university workshop on **machine learning in
cybersecurity**. It is a learning exercise, not a security tool.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/kafkaesqueismn/network-scan-detector/blob/main/Network_Scan_Detector.ipynb)

## What the notebook covers

1. Generating 1,000 synthetic connections. Normal and scan-like traffic come
   from different distributions.
2. Labelling them with a risk-points rule and flipping about 5% of the labels
   to simulate imperfect ground truth
3. Exploring class balance and feature averages, with discussion questions
4. An 80/20 stratified train/test split
5. Training a `DecisionTreeClassifier` (`max_depth=4`, `min_samples_leaf=10`)
6. Evaluating it with accuracy, a classification report and a confusion matrix,
   plus what false positives and false negatives mean for a SOC
7. Visualising the decision tree and feature importances
8. A `check_connection()` helper that returns a prediction, a score and an
   illustrative response tier
9. Three scenarios (normal browsing, an aggressive scan and an authorised
   internal vulnerability scan), a "your turn" cell and a challenge to evade
   the model with a low-and-slow scan

### Features used

| Feature | Range |
|---|---|
| `ports_contacted_per_minute` | 1–500 |
| `unique_destination_ips` | 1–100 |
| `syn_only_ratio` | 0.0–1.0 |
| `avg_packet_size_bytes` | 40–1500 |
| `failed_connection_ratio` | 0.0–1.0 |
| `hour_of_day` | 0–23 |

## Running it in Google Colab

1. Click the **Open in Colab** badge above. Alternatively, go to
   [colab.research.google.com](https://colab.research.google.com), choose
   **File → Open notebook → GitHub** and paste this repository's URL.
2. Choose **Runtime → Run all**. You can also run the cells one at a time with
   **Shift + Enter**.

You don't need to install anything. The notebook only uses pandas, numpy,
matplotlib and scikit-learn, which are already available in Colab. It sets
`np.random.seed(42)`, so every run gives the same results.

### Running locally (optional)

```bash
pip install pandas numpy matplotlib scikit-learn jupyter
jupyter notebook Network_Scan_Detector.ipynb
```

## Limitations

- **All data is synthetic.** It contains no real IP addresses, hosts or
  traffic. The distributions are invented and are much cleaner than real
  network data.
- **The labels come from a rule.** They are not verified incidents, so the model
  mostly learns that rule back. The high accuracy reflects this and is not
  real-world detection performance.
- **One feature dominates.** In this synthetic data `syn_only_ratio` separates
  the classes almost on its own, which makes the model easy to evade.
- **The model sees behaviour, not intent.** It flags authorised scanners and
  misses slow, stealthy scans.
- **This is not for real use.** Don't deploy it or use it to make decisions about
  real networks. In practice, ML is just one layer alongside IDS rules, rate
  limiting, allowlists of authorised scanners and human review.
