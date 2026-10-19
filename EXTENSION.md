E2 Bonus: MPI Deadlock Threshold Experiment


Objective


Investigate how image width affects the completion of the naive MPI halo-exchange program.


Experimental Setup


MPI: Open MPI 4.1.4


Python interface: mpi4py


MPI ranks: 6


Image height: 300 pixels


Timeout: 10 seconds per test


Program: img_mpi_v3_naive_halo.py


Results: Completed Successfully


Exit code: 0


Widths tested:


256, 1024, 2048, 2560, 3072, 3584, 3840, 3904, 3968, 4000, 4032, 4040


Results: Timed Out


Exit code: 124


Widths tested:


4042, 4044, 4046, 4048, 4056, 4060, 4064, 4096, 8192


Analysis


Exit code 0 means the program completed successfully. Exit code 124 means the timeout stopped the program before completion.


Width 4040 completed, while width 4042 timed out in this test environment. This is an observed transition for this configuration, not a universal MPI threshold.


The results are consistent with a possible deadlock caused by blocking communication in the naive halo-exchange implementation.


Verification


The separate test of img_mpi_v2_halo.py at width 8192 completed successfully.


Comparison with the serial reference:


np.array_equal: True


Differing pixels: 0 / 2457600


Result: Exact match.


Conclusion


Small inputs completing successfully does not guarantee that larger inputs will complete with the same MPI communication pattern.


The corrected halo-exchange implementation passed the width-8192 comparison with zero differing pixels.


These experiments document the observed behavior and the verification of the corrected implementation.


