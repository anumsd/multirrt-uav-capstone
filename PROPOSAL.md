# Capstone Proposal (Track A: Reproduce and Extend)

**Student:** Anum Shahzad | **Repository:** https://github.com/anumsd/multirrt-uav-capstone | **Date:** 21 November 2026

## 1. Benchmark paper
- T. H. Khuat, D.-N. Bui, H. T. T. Nguyen, M. L. Trinh, M. T. Nguyen, M. D. Phung, "Multi-goal Rapidly Exploring Random Tree with Safety and Dynamic Constraints for UAV Cooperative Path Planning," *IEEE Transactions on Vehicular Technology*, vol. 74, no. 9, 2025, doi:10.1109/TVT.2025.3560658.
- Free version: arXiv:2504.11823 (https://arxiv.org/abs/2504.11823).
- Venue rank: IEEE TVT, Q1 in all five of its Scimago categories (2025), including Automotive Engineering and Electrical and Electronic Engineering; SJR 2025 = 2.052, H-index 254.
- Citations: 36 (Google Scholar, 21 November 2026).
- Code (MIT licence, Python): https://github.com/duynamrcv/multi-target_RRT (linked in the paper's first-page footnote). No dataset needed; the four test maps are in the code.

## 2. Result to reproduce
- Figs 13, 14 and 15 of the paper (Section V-C), scenarios 1 and 2: MultiRRT vs RRT* vs RRT*-Smart on path length F_L (Eq. 35), smooth score F_S (mean turning angle, Eq. 36) and computation time F_T, 10 runs per method per scenario, as in the paper.
- **Paper's stated claims (my formal check):** MultiRRT achieves the best score on all three metrics, and its smooth score is "at least three times better" than the other algorithms (Section V-C). The paper also reports MultiRRT's total execution time as under 1 s.
- **My own readings of the plots (not claims made by the paper):** the smooth score looks about 5 to 8 times lower than both baselines, and in scenario 1 the length difference between MultiRRT and RRT*-Smart looks like a near-tie, within the spread of the boxes. I will test these as observations, separately from the paper's stated claims.
- Approximate values read by eye from the paper's Figs 13 to 15 (central line of each box):
  - Scenario 1: F_L about 1330 m (RRT*), 1160 m (RRT*-Smart), 1150 m (MultiRRT); F_S about 0.67, 0.85, 0.13; F_T about 3 s, 38 s, under 1 s.
  - Scenario 2: F_L about 1290 m, 1110 m, 1050 m; F_S about 0.67, 0.76, 0.10; F_T about 3.5 s, 39 s, 0.5 s.
- **Statistic check:** the paper says its box plots show the minimum, maximum, average and standard deviation. Before comparing, I will check whether the central line is the median or the mean, and use the same statistic for my results.
- I will compare rankings and ratios, since absolute times depend on hardware.
- **Discrepancy:** the paper states a maximum turning angle of 75 degrees (Table I), while the released code uses 60 degrees (to be confirmed in the code). I will use the released 60 degrees as the main setting, run 75 degrees once for comparison, and discuss any difference.
- **F_S check:** F_S is the mean turning angle with no unit stated (values of 0.1 to 0.85 suggest radians). I will check in the code whether F_S is computed on the raw waypoints or on the Bezier-smoothed path, and use the same definition for all three methods.

## 3. Research question
How do the maximum turning angle (30, 45, 60 and 75 degrees) and the width of a narrow passage (5 widths) affect MultiRRT's success rate, path length and smoothness? The paper reports no success rates (all its runs reach the targets), so this analysis is new.

**Extension design:**
- A new test map with two staggered walls, each with one gap of adjustable width, so the paths must turn through the gaps.
- 30 seeded trials per (width, angle) pair.
- A failed run (not all three goals reached within 5000 iterations) counts as a failure and is never silently retried. The 5000-iteration limit and the step size delta = 50 follow the paper.
- RRT* is run on the same maps as an unconstrained reference.
- The turning angle is the deviation from straight (Eqs. 2 and 20), so 30 degrees is the most restrictive setting and 75 degrees the least.
- Gap width is defined after obstacle inflation by the UAV radius, as in the paper's collision checking. Widths will be chosen relative to delta and the inflated radius so that the widest gap is clearly passable.
- A geometric feasibility check on the hardest (width, angle) pairs will separate maps that are impossible to traverse from genuine algorithm failures.
- In the analysis I will discuss the Bezier safe zone (Definition 1, Theorem 1), whose radius is the distance to the nearest obstacle and so shrinks in narrow gaps.

## 4. Data and compute
- Pure Python simulation (NumPy, SciPy, Matplotlib). No GPU and no dataset.
- Runs on Colab or Kaggle notebooks (CPU). My estimates of single-run time are about 1 to 2 s for MultiRRT, about 10 s for RRT* and about 2 min for RRT*-Smart. The paper reports MultiRRT's total time as under 1 s, so these estimates will be confirmed by a small pilot before the full grid. Narrow-passage failures may run longer.
- All results come from my own code, with fixed random seeds, saved to the repo.

## 5. Plan
- **By Fri 4 Dec (checkpoint):** repo set up; analysis scripts fixed for the RRT*/RRT*-Smart/MultiRRT comparison; Figs 13 to 15 reproduced for scenarios 1 and 2 with 10 seeded runs each; comparison table of the paper's numbers against mine.
- **By Fri 18 Dec (final):** corridor map built and piloted; full width-by-angle grid run; success rate, length and smoothness analysed; IEEE-format paper (4 to 6 pages, 8+ real references I have read, for example RRT, RRT*, RRT*-Smart, Theta-RRT and FN-RRT), README with code link and Python skills profile link.
- **Fallbacks:** if some baseline will not reproduce, report the difference and explain it (still counts as a result). If the full grid is too slow, reduce to 3 widths by 3 angles with 20 trials each. If the corridor map fails, sweep the turning angle alone on the paper's scenarios.
