# Lab 4 Analysis

## Recorded results

Run with different process counts:
```bash
mpiexec -n P python lab4_pi_integration.py  # P = 1, 2, 4, ...
```

Number of Processes (P) | Pi Approximation | Execution Time (s)
--- | --- | ---
1 | 3.141592653589987 | 0.4167
2 | 3.141592653589722 | 0.2545
4 | 3.141592653589782 | 0.2467
8 | 3.141592653589801 | 0.2724

## Analysis questions

1. **Collectives used**  
   Which MPI collectives did you use and what was the purpose of each in this algorithm?
   The `comm.bcast` was used for One-to-All distribution of N trapezoids from the root process (rank 0 in our case) to other processes.
   The `comm.reduce` was used for All-to-One distribution of the global sum (MPI.SUM) partial integrals computed in each rank to the root process.

2. **Data distribution**  
   How was the integration interval decomposed among processes without an explicit scatter, and what decomposition pattern does this resemble (block, cyclic, etc.)?
   The used distribution patter is block decomposition. If N ranks, rank 0 handles trapezoids [0, local_n], rank 1 handles [local_n, 2*local_n], and so on - giving each process a distinct contigous chunk of the total problem. The remainder of N/p was distributed by assigning one extra trapezoid to the first remainder ranks, so they each get intervals_per_process + 1 while the rest get intervals_per_process.

3. **reduce vs allreduce**  
   What would change if you used `comm.allreduce` instead of `comm.reduce`, and when might `allreduce` be preferred?
   The `comm.allreduce` collective communication method function similar to `comm.reduce` (All-to-One) plus `comm.bcast` (broadcasting the result to all processes). I think it would be useful in cases where each process has to have the result available in its local memory.

4. **Performance**  
   Did execution time decrease as the number of processes increased? Explain your observation in terms of computation vs communication (broadcast and reduction).
   Yes, execution time decreased significantly from P=1 to P=2 (0.4167s to 0.2545s), showing a clear parallel speedup as the computational workload was halved across two processes. From P=2 to P=4 the improvement became marginal (0.2545s to 0.2467s), and at P=8 the time actually increased slightly (0.2724s) which I see as the sign of the communication overhead prevailing above the computational gain. In my opinion, this finding is strongly linked to Amdahl's Law, as behind a certain number of processes the overhead introduced by message passing (distributed-memory systems but also shared-memory systems in case of non-uniform memory access systems and multiprocessing communication) or synchronization (threading) limits the speed-up capacity.