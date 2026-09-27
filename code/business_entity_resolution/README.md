# Amazon ML Challenge 2026 — Business Entity Resolution

## Reproduction
1. Start a Google Colab runtime with Python 3.
2. Mount Google Drive.
3. Place the challenge `dataset/` under the configured challenge root.
4. Run the supplied notebook `AmazonMLChallenge_EntityResolution_FINAL.ipynb` from top to bottom.
5. The pipeline normalizes all three sources, performs multi-stage blocking, applies missing-pair rescue blocks, builds supervised training pairs from the supplied training ground truth, trains a CatBoost pairwise classifier, and writes the final TSV files under `output/`.

## Candidate generation
The blocking pipeline uses normalized name/address exact blocks, legal-suffix-stripped names, country/house combinations, postal/name and house/name prefix/suffix blocks, sorted token signatures, rare-token rescue, character n-gram rescue, and constrained fuzzy retrieval. Candidate generation is designed to reduce the search space before pairwise scoring.

## Matching model
A CatBoost binary classifier scores candidate pairs using exact agreements and string-similarity features for name/address plus country, postal code, house number, and composite blocking signals. The decision threshold is selected against held-out training data using F0.5.

## Submission outputs
- `output/matching_results.tsv`
- `output/candidate_pairs.tsv`

## Compliance
The pipeline uses only the challenge-provided data and training ground truth. It does not use external business databases, APIs, geocoding services, or external entity-resolution services.
