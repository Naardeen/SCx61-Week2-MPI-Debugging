# Knowledge Check: MPI Deadlock Debugging

## 1. What is a deadlock in MPI?

A deadlock occurs when two or more processes wait indefinitely for communication or synchronization that cannot progress under the current execution pattern.

## 2. Why can a blocking MPI program work on small inputs but hang on larger inputs?

MPI implementations may buffer some small messages, allowing a send to complete before the matching receive is posted. Larger messages may require the receiver to participate before the send can complete. If all involved processes wait in incompatible blocking operations, the program can deadlock.

This is a possible explanation for the observed behavior, not proof that message size was the only factor in this experiment.

## 3. What is halo exchange?

Halo exchange communicates boundary rows or cells between neighboring processes. Each process uses this neighboring data to process pixels near the boundaries of its assigned image region.

## 4. How does MPI_Sendrecv help?

`MPI.Sendrecv` combines a send and a receive into one communication operation. It can avoid certain deadlocks caused by ordering blocking sends before receives, provided that the communication partners, tags, and buffers are correctly configured.

It does not automatically guarantee that every MPI program is deadlock-free.

## 5. Why are matching tags important?

The sender's tag and communicator must match the corresponding receive operation. Incorrect partners, tags, or message sizes can cause communication errors or processes to wait for messages that never arrive.

## 6. Why compare parallel output with a serial reference?

A parallel program can finish without producing correct results. Comparing its output with a trusted serial implementation checks whether the parallel computation produces the expected result for the tested input.

## 7. What did the width experiment establish?

In the recorded environment, width 4040 completed and width 4042 timed out. This established an observed transition for the tested configuration, not a universal MPI message-size threshold.

## 8. What did the width-8192 verification establish?

The corrected implementation produced an array exactly equal to the serial reference, with zero differing pixels out of 2,457,600 for that test.

This supports correctness for that particular input and configuration, but does not prove correctness for all inputs.

## 9. Why was the eager-limit configuration not accepted as a general fix?

Although the configuration allowed the width-8192 test to complete, the width-20000 test still timed out. Therefore, it did not resolve the problem for every tested size.

## 10. What is the main lesson?

Distributed programs must be tested with different input sizes, and communication correctness should be verified independently of whether the program terminates successfully.
