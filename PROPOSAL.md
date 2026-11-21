# Capstone Proposal (Track A: Reproduce and Extend)

**Student:** Anum Shahzad | **Repository:** https://github.com/anumsd/multirrt-uav-capstone | **Date:** 19 November 2026

## 1. Benchmark paper
- T. H. Khuat, D.-N. Bui, H. T. T. Nguyen, M. L. Trinh, M. T. Nguyen, M. D. Phung, "Multi-goal Rapidly Exploring Random Tree with Safety and Dynamic Constraints for UAV Cooperative Path Planning," *IEEE Transactions on Vehicular Technology*, 2025, doi:10.1109/TVT.2025.3560658.
- Free version: arXiv:2504.11823 (https://arxiv.org/abs/2504.11823).
- Venue rank: IEEE TVT, Q1 in all five of its Scimago categories (2025), including Automotive Engineering and Electrical and Electronic Engineering; SJR 2025 = 2.052, H-index 254
- Citations: 36 (Google Scholar, 21 November 2026).
- Code (MIT licence, Python): https://github.com/duynamrcv/multi-target_RRT. No dataset needed; the four test maps are in the code.

## 2. Result to reproduce
- Figs 13, 14 and 15 of the paper, scenarios 1 and 2: MultiRRT vs RRT* vs RRT*-Smart on path length F_L, smooth score F_S and computation time F_T, 10 runs per method per scenario, as in the paper.
- Paper's claims to check: MultiRRT has the lowest median computation time and by far the best smooth score (about 5 to 8 times lower than both baselines in the paper's figures), and the lowest median path length, with a near-tie against RRT*-Smart in scenario 1.
- Approximate values read from the paper's Figs 13 to 15 (median of each box, read from the plots by eye):
Scenario 1: F_L about 1330 m (RRT*), 1160 m (RRT*-Smart), 1150 m (MultiRRT); F_S about 0.67, 0.85, 0.13; F_T about 3 s, 38 s, under 1 s.
Scenario 2: F_L about 1290 m, 1110 m, 1050 m; F_S about 0.67, 0.76, 0.10; F_T about 3.5 s, 39 s, 0.5 s.
I will compare rankings and ratios, since absolute times depend on hardware. In scenario 1 the length difference between MultiRRT and RRT*-Smart is within the spread of the boxes.
- Note: the released code uses a maximum turning angle of 60 degrees, while the paper text says 75 degrees. I will use the released 60 degrees as the main setting, run 75 degrees once for comparison, and discuss any difference.

## 3. Research question
How do the maximum turning angle (30, 45, 60 and 75 degrees) and the width of a narrow passage (5 widths, from wide down to near the UAV size) together affect MultiRRT's success rate, path length and smoothness, and does a tight turning limit make narrow passages fail sooner?

Extension design: a new test map with two staggered walls, each with one gap of adjustable width, so the paths must turn through the gaps. 30 seeded trials per (width, angle) pair. A failed run (not all three goals reached within 5000 iterations) counts as a failure and is never silently retried. RRT* is run on the same maps as an unconstrained reference.

## 4. Data and compute
- Pure Python simulation (NumPy, SciPy, Matplotlib). No GPU and no dataset.
- Runs on Kaggle notebooks (CPU). Single MultiRRT run takes about 1 to 2 s on the paper's maps; RRT* about 10 s; RRT*-Smart about 2 min. Narrow-passage failures may run longer, so I will time a small pilot before the full grid.
- All results come from my own code, with fixed random seeds, saved to the repo.

## 5. Plan
- **By Fri 4 Dec (checkpoint):** repo set up; analysis scripts fixed for the RRT*/RRT*-Smart/MultiRRT comparison; Figs 13 to 15 reproduced for scenarios 1 and 2 with 10 seeded runs each; comparison table of the paper's numbers against mine.
- **By Fri 18 Dec (final):** corridor map built and piloted; full width-by-angle grid run; success rate, length and smoothness analysed; IEEE-format paper (4 to 6 pages, 8+ real references I have read), README with code link and Python skills profile link.
- **Fallbacks:** if some baseline will not reproduce, report the difference and explain it (still counts as a result). If the full grid is too slow, reduce to 3 widths by 3 angles with 20 trials each. If the corridor map fails, sweep the turning angle alone on the paper's scenarios.
