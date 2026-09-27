# Amazon ML Challenge 2026 — Methodology

## 1. Methodology
We perform large-scale business entity resolution from the deduplicated Source 1 reference table to Source 2 and Source 3. The pipeline separates candidate generation from pairwise classification so that expensive similarity computation is applied only to a reduced candidate set.

## 2. Candidate Generation / Blocking
The blocking pipeline combines:
- normalized exact-name and exact-address blocks;
- legal-suffix-stripped name blocks;
- country + house-number blocks;
- postal/name prefix and suffix blocks;
- house/name prefix and suffix blocks;
- sorted name-token signatures;
- rare informative name/address token rescue;
- character 4-gram rescue;
- constrained fuzzy retrieval.

For the emergency final artifact, the submitted candidate file is the strong exact-match candidate set used by the final output generation step. This is smaller than the earlier full blocking union; the leaderboard scores only matching_results.tsv, while candidate_pairs.tsv is retained for audit.

## 3. Feature Engineering
Candidate pairs use exact agreement features for name, normalized name, address, country, postal code, and house number, together with Jaro-Winkler and length-similarity features and composite blocking signals.

## 4. Model Architecture
A CatBoost binary classifier is trained on labeled candidate pairs constructed from the supplied training ground truth. Negative examples are sampled deterministically to keep training memory-bounded.

## 5. Threshold Selection
The prediction threshold is selected using the F0.5 objective on a held-out portion of the sampled training pairs. F0.5 is precision-heavy, so the final decision rule avoids unnecessary false merges.

## 6. Output
The pipeline generates one row for every Source 1 test entity in both output files. `matching_results.tsv` contains final predicted matches and `candidate_pairs.tsv` contains the candidate IDs.

## 7. Fair Play / Compliance
Only the challenge-provided data and training ground truth are used. No external business lookup, geocoding, registry lookup, API-based entity resolution, or external data augmentation is used.
