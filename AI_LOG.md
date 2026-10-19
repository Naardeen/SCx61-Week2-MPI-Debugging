# AI Usage Log

## Purpose

AI assistance was used as a learning and debugging aid during the SCx61 Week 2 MPI deadlock investigation.

## How AI Was Used

AI assistance helped with:

- Explaining blocking MPI communication and possible deadlock scenarios.
- Suggesting diagnostic checks for hostname resolution, network connectivity, and SSH access.
- Helping organize the experimental results and document the investigation.
- Explaining why an alternative halo-exchange implementation using `MPI.Sendrecv` should be checked against a serial reference.
- Improving the structure and clarity of the project documentation.

## Human Verification

The experiments were run in the Docker-based MPI cluster using Open MPI 4.1.4 and mpi4py.

The recorded observations included:

- Successful and timed-out image-width tests.
- Hostname resolution, ping, and SSH checks.
- Comparison of the corrected implementation against a serial reference at width 8192.
- An additional width-20000 timeout experiment.

The reported measurements are limited to the configurations actually tested. AI-generated explanations were treated as hypotheses to investigate rather than proof of the cause.

## Limitations

AI assistance does not replace understanding the MPI communication pattern or verifying the program experimentally. The student is responsible for reviewing the code, understanding the results, and following the course's academic-integrity rules.

No performance improvement is claimed without supporting measurements.
