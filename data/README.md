# PacAbide — Flow feature CSVs used in the experiments

This folder contains the flow-feature CSVs that were actually used by the experiments in
*PacAbide: Semi-Supervised Learning-Based Network Intrusion Detection via Packet-Abiding
Transmission Augmentation*. All files were extracted with **CICFlowMeter V4.0**
(flow timeout 120 s, activity timeout 5 s, jnetpcap 1.4.1) from the original packet
captures; the augmented views were produced by the packet-level transport augmentation
described in Section 3.2 of the paper and then re-extracted with the same extractor.

Each file is gzip-compressed individually. To decompress:

```bash
gunzip -k data/cicids2017/*/*.gz data/csecicids2018/*/*/*.gz 2>/dev/null || \
find data -name '*.gz' -exec gunzip -k {} +
```

## Layout

```
data/
  cicids2017/                       # CIC-IDS2017, five capture days (subsampled, seed 42)
    Monday|Tuesday|Wednesday|Thursday|Friday/
      orig.flows.csv.gz             # original view
      rc_weak.flows.csv.gz          # transport shaping, weak setting  (p=0.05, w=2, sigma=200us, c=+-500us)
      rc_strong.flows.csv.gz        # transport shaping, strong setting (p=0.30, w=8, sigma=500us, c=+-1ms)
  csecicids2018/                    # CSE-CIC-IDS2018, the two days used in Experiment 5
    Wednesday-14-02-2018/
      orig.flows.csv.gz             # original view
      ts_strong.flows.csv.gz        # transport shaping, strong setting
    Thursday-15-02-2018/
      orig.flows.csv.gz
      ts_strong.flows.csv.gz
```

## Relation to the paper

- These CSVs are the extractor outputs **before** label mapping, view alignment,
  deduplication, and the 70:30 split (Section 3.1 / Section 4.1 of the paper). The final
  CIC-IDS2017 common set derived from them contains 460,949 flows (322,664 train /
  138,285 test, attack share 41.6%); the CSE-CIC-IDS2018 common set contains 280,099
  flows (28.41% attack).
- `orig` + `rc_strong` form the PacAbide condition (C5); `orig` alone is the supervised
  baseline (C1); `rc_weak` + `rc_strong` form the symmetric placement (C4).
- CSE-CIC-IDS2018 labels were mapped from the re-labelled release of Liu et al. (see
  citation below); the label mapping itself is not part of these CSVs.
- The views of the other augmentation operators compared in Experiments 3 and 4
  (Field editing, Packet composition, Feature corruption) are not included here for
  size reasons; they can be regenerated with the released augmentation code.

## Source datasets and attribution

The underlying packet captures are from the Canadian Institute for Cybersecurity (CIC),
University of New Brunswick, and are redistributed here in derived (feature-extracted)
form with attribution:

- CIC-IDS2017: <https://www.unb.ca/cic/datasets/ids-2017.html> — Sharafaldin, I.,
  Lashkari, A. H., Ghorbani, A. A. (2018). *Toward Generating a New Intrusion Detection
  Dataset and Intrusion Traffic Characterization.* ICISSP 2018.
- CSE-CIC-IDS2018: <https://www.unb.ca/cic/datasets/ids-2018.html>
- Re-labelled CSE-CIC-IDS2018 flows (label source for Experiment 5): Liu, L.,
  Engelen, G., Lynar, T., Essam, D., Joosen, W. (2022). *Error Prevalence in NIDS
  Datasets: A Case Study on CIC-IDS-2017 and CSE-CIC-IDS-2018.* IEEE CNS 2022.
- CICFlowMeter: Lashkari, A. H. et al., <https://github.com/ahlashkari/CICFlowMeter>

If you use these files, please cite the PacAbide paper and the original dataset papers
above.
