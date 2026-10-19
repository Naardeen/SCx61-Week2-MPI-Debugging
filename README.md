# SCx61 Week 2 — MPI Deadlock Debugging

## Overview

This repository documents the E2 bonus experiment for the SCx61 Distributed Computing Lab. The experiment investigates how image width affects a naive MPI halo-exchange implementation and verifies a corrected implementation using `MPI.Sendrecv`.

## Environment

- Open MPI 4.1.4
- Python with `mpi4py` and NumPy
- Docker Compose cluster with `node0`, `node1`, and `node2`
- 6 MPI ranks
- Synthetic grayscale images, 300 pixels high

## Files

| File | Description |
|---|---|
| `img_mpi_v3_naive_halo.py` | Naive blocking halo-exchange implementation used in the deadlock experiment |
| `img_mpi_v2_halo.py` | Halo-exchange implementation using `Sendrecv` |
| `make_test_image.py` | Generates deterministic synthetic test images |
| `verify_fix.sh` | Runs the corrected implementation and compares it against a serial reference |
| `EXTENSION.md` | Experimental results, analysis, and conclusions |

## Experiment 1: Image Width and Completion

Each test used a 10-second timeout.

- **Completed:** widths from 256 through 4040 among the tested values.
- **Timed out:** widths from 4042 through 8192 among the tested values.

The observed transition occurred between widths 4040 and 4042 in this particular environment. It is not a universal MPI threshold.

The results are consistent with a possible deadlock associated with the naive blocking communication pattern.

## Experiment 2: Verification of the Corrected Implementation

The corrected implementation was tested with an image of size 8192 × 300.

Results:

- `np.array_equal: True`
- Differing pixels: `0 / 2457600`

The parallel output exactly matched the serial reference for this test.

## Experiment 3: MCA Parameter Test

Increasing the Open MPI eager limit allowed the 8192-wide test to complete, but the 20000-wide test still timed out.

This indicates that the parameter change did not provide a general solution in the tested environment.

## Conclusion

The experiments demonstrate that successful execution on smaller inputs does not guarantee successful execution on larger inputs.

The corrected halo-exchange implementation passed the 8192 × 300 comparison with zero differing pixels.

## Requirements

Python, NumPy, Pillow, `mpi4py`, Open MPI, and the lab's `hostfile`, `filter_ref.py`, and `compare.py` are required for the relevant experiments.

## Reproducibility

Use the provided scripts within the original SCx61 Week 2 lab environment. Results may vary with MPI configuration, system resources, and execution environment.
