# Superdense syndrome extraction for non-self-dual codes

Course project (PQC). We test whether the superdense trick (measuring an X check and a Z check with one Bell pair of ancillas) lowers the logical error rate for codes whose X and Z checks do not have the same support.

- **Code A:** rotated surface code, d = 3 and 5
- **Code B:** [[72,12,6]] bivariate bicycle (BB) code
- **Noise:** uniform circuit-level depolarizing noise (CNOT, reset, measurement and idle errors, all with strength p)
- **Decoder:** BP-OSD (`ldpc` package) on the Stim detector error model

## Contents

| File | What it is |
|---|---|
| `PQC_Project_revised.ipynb` | All code and results: circuits, benchmarks, circuit search |
| `docs/PQC_Project_Overview.docx` | Written overview of the project and its results |
| `requirements.txt` | Package versions used |

## Notebook structure

1. **Task 2, circuits.** GF(2) helpers and logical operators, the two codes, the superdense pairing, and the circuit builder (`build`). It builds bare-ancilla (interleaved) and superdense memory experiments.
2. **Task 3, benchmarks.**
   - The BP-OSD tools and a parallel runner (`run_point`).
   - The surface-code sweep, the BB sweep, a decoder-strength check and a circuit-distance check.
3. **Task 4, alternative circuits.**
   - Search and validation functions, and a 9-item checklist.
   - Surface code: two finalists, C1 and C2, compared by LER.
   - BB code: a search over shift pairings with translation-invariant schedules, then two finalists, B1 and B2, compared by LER.

## Main results

- The superdense circuit measures the correct check eigenvalues whenever the paired X and Z checks overlap evenly (Pauli-tracking proof in the overview).
- With the initial circuits, superdense extraction is worse than bare extraction on both codes. On the surface code it also lowers the circuit distance (5 to 3 at d = 5) and the threshold.
- Task 4, surface code: restoring the distance costs extra layers, and the LER does not improve.
- Task 4, BB code: a translation-invariant schedule (6 CNOT layers) lowers the superdense LER about 4× at p = 2e-3. With idle noise switched off it matches the bare circuit. With full noise it is still about 2× worse.

## How to run

```bash
pip install -r requirements.txt
```

Run the notebook cells from top to bottom. The functions are defined in the notebook itself (there are no separate modules).

- The surface-code cells take minutes on one core.
- The BB benchmark cells use `run_point` with several worker processes. Each worker needs about 0.5–0.7 GB of RAM, and the BB cells take about 3 hours in total with 6 workers.
- The parallel runner relies on forked worker processes, so it needs Linux or macOS. On Windows, run the notebook on a Linux machine or server.
- All random choices are seeded, so the circuits and the finalists (`specs`, `bb_specs`) are reproducible.

## Limitations

- Only one noise model was used (idle noise at full strength p), with Z-memory LER only.
- The circuit distances come from Stim's bounded search, so they are upper bounds, not proofs.
- The BB LER counts a failure of any of the 12 logical qubits.
- The superdense layout assumes one extra ancilla–ancilla coupler per X–Z pair. For the BB code this coupler is non-local.

## References

1. Gidney & Jones, *New circuits and an open source decoder for the color code*, arXiv:2312.08813
2. *Denser planar color codes*, arXiv:2609.21376
3. *Barbell codes*, arXiv:2606.06062
4. Fowler et al., *Surface codes: towards practical large-scale quantum computation*, arXiv:1208.0928
5. Bravyi et al., *High-threshold and low-overhead fault-tolerant quantum memory*, arXiv:2308.07915
