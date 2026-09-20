# SubspaceResolve

SubspaceResolve is an early open-source prototype for studying recovery of near-degenerate interior eigenspaces in variational quantum calculations. In this regime, a variational method may recover the correct low-dimensional subspace while still failing to identify stable individual states or resolve their small internal energy splitting.

The method first recovers the target subspace by minimizing a folded objective based on `H²`. It then diagonalizes the original Hamiltonian inside the recovered subspace to reconstruct the signed near-zero energies and their internal splitting.

The current evidence is a small SSH ideal-statevector calculation that reproduces the finite-size near-zero pair across chain lengths `N = 8, 10, 12, 14, 16, 18`. The reported quantities are the target-subspace fidelity, reconstructed internal splitting, normalized splitting error, and optimizer convergence information.

## Status

**Status:** This repository contains a preliminary ideal-statevector prototype. It does not yet represent a finite-shot, noisy-simulator, circuit-level, or quantum-hardware validation.

## Contents

- `notebooks/SubspaceResolve_SSH_Ideal_Prototype.ipynb`: complete reference workflow
- `results/ssh_ideal_reference.csv`: spectral, subspace, splitting, and selected optimizer results
- `figures/ssh_ideal_reference.png`: exact and reconstructed near-zero pair versus chain length

## Run

Create a Python environment, install the dependencies, start Jupyter, and run the notebook from top to bottom.

```bash
python -m pip install -r requirements.txt
jupyter notebook
```

The notebook creates the CSV file and the figure automatically.

## Verification

The notebook was executed and compared with reference calculation.

- Maximum difference in the exact near-zero energies for `N = 8, 10, 12, 14`: `5.0e-16`
- Maximum difference in the reconstructed energies for `N = 8, 10, 12, 14`: `1.1e-16`
- Maximum difference in reconstructed splitting for `N = 8, 10, 12, 14, 16, 18`: `4.2e-14`
- Maximum difference in target-subspace fidelity: `1.7e-11`
- Minimum Python target-subspace fidelity: `0.999999999984`
- Maximum Python normalized splitting error: `1.41e-14`
- Both selected optimizer stages reported successful convergence for every chain length.

These differences are at the level of floating-point and optimizer roundoff. The exact and reconstructed splittings both decrease monotonically with chain length.

## Method

For an open SSH chain with intracell hopping `v = 0.5` and intercell hopping `w = 1.0`, the notebook:

1. constructs the one-particle Hamiltonian;
2. obtains the exact pair closest to zero;
3. builds left-edge and right-edge initial states from the SSH decay profile;
4. minimizes the folded objective based on `H²`;
5. uses overlap deflation to recover a second independent state;
6. orthonormalizes the recovered two-state basis;
7. projects `H` into that basis and jointly sorts its eigenvalues and eigenvectors;
8. evaluates subspace fidelity and internal splitting error;
9. records optimizer success, status, message, iterations, function evaluations, and final cost.

## Licence

Apache License 2.0.
