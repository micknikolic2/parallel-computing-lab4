# Lab 4 Analysis

## Recorded results

Run with different process counts:
```bash
mpiexec -n P python lab4_pi_integration.py  # P = 1, 2, 4, ...
```

Number of Processes (P) | Pi Approximation | Execution Time (s)
--- | --- | ---
1 | [enter value] | [enter time]
2 | [enter value] | [enter time]
4 | [enter value] | [enter time]
[add more rows if tested] |  | 

## Analysis questions

1. **Collectives used**  
   Which MPI collectives did you use and what was the purpose of each in this algorithm?

2. **Data distribution**  
   How was the integration interval decomposed among processes without an explicit scatter, and what decomposition pattern does this resemble (block, cyclic, etc.)?

3. **reduce vs allreduce**  
   What would change if you used `comm.allreduce` instead of `comm.reduce`, and when might `allreduce` be preferred?

4. **Performance**  
   Did execution time decrease as the number of processes increased? Explain your observation in terms of computation vs communication (broadcast and reduction).
