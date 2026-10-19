# Incident Report: MPI Halo-Exchange Deadlock

## 1. Incident Summary

During the SCx61 Week 2 distributed computing lab, an MPI image-processing job completed successfully on smaller images but timed out on larger images.

The investigation focused on blocking MPI communication during halo exchange between neighboring ranks.

## 2. Environment

- Operating system: Windows host running Docker containers
- MPI implementation: Open MPI 4.1.4
- Python interface: mpi4py
- MPI ranks: 6
- Test image height: 300 pixels
- Timeout for width experiments: 10 seconds
- Cluster: node0, node1, node2

## 3. Observed Symptoms

The naive halo-exchange implementation completed for the following tested image widths:

256, 1024, 2048, 2560, 3072, 3584, 3840, 3904, 3968, 4000, 4032, 4040.

The following tested widths timed out:

4042, 4044, 4046, 4048, 4056, 4060, 4064, 4096, 8192.

Exit code 0 indicated successful completion. Exit code 124 indicated that the timeout terminated a test before it completed.

The transition between widths 4040 and 4042 was observed in this specific environment. It should not be treated as a universal MPI threshold.

## 4. Investigation

### Hypothesis 1: DNS or network connectivity failure

This hypothesis was investigated by resolving the node hostnames and testing connectivity.

Observed results:
- node1 resolved to 172.18.0.3.
- node2 resolved to 172.18.0.4.
- Both nodes responded to ping with 0% packet loss in the recorded test.

These results did not support a general DNS or network connectivity failure as the explanation for the observed timeout.

### Hypothesis 2: SSH connectivity failure

SSH access was tested using `ssh node1 hostname`.

Observed result: the command returned `node1`.

This did not support an SSH connectivity failure as the primary explanation.

### Hypothesis 3: Blocking communication during halo exchange

The observed behavior was consistent with a deadlock involving blocking MPI sends and receives. Depending on message size and MPI buffering behavior, processes can wait for matching communication operations rather than making progress.

The width-dependent transition supported further investigation of the communication pattern. It did not, by itself, prove the precise internal cause.

## 5. Corrective Action and Verification

A corrected halo-exchange implementation using `MPI.Sendrecv` was tested with an image width of 8192.

Comparison against the serial reference produced:

- `np.array_equal`: True
- Differing pixels: 0 / 2,457,600
- Result: Exact match

This verified the output for the tested configuration and input. It does not establish correctness for every possible input or cluster configuration.

## 6. Additional Experiment

A test involving an MPI eager-limit configuration completed at width 8192, but a width-20000 test still timed out with exit code 124.

Therefore, the configuration change was not considered a general solution.

## 7. Lessons Learned

- Successful execution on small inputs does not guarantee successful execution on larger inputs.
- Blocking MPI communication must be designed so that matching sends and receives can make progress.
- Network and SSH checks help eliminate alternative explanations but do not independently prove the cause of an MPI deadlock.
- A corrected parallel implementation should be compared against a serial reference.
- Performance and correctness claims should be limited to configurations that were actually tested.

## 8. Evidence

Attach or link the actual terminal output or cropped screenshots for:
- A successful small-width run.
- A timed-out large-width run.
- The hostname and connectivity checks.
- The exact-match verification at width 8192.
- The width-20000 timeout experiment.

Only include evidence that was actually captured. Do not invent screenshots or claim that missing evidence exists.
