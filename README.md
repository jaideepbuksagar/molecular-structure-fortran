# Molecular Structure Modelling — Graph-Theoretic Schrödinger Solver

Numerical modelling of molecular electronic structure by solving the **multi-body Schrödinger equation** using a graph-theory framework. Implemented in FORTRAN for computational efficiency.

> B.Tech. coursework project, Department of Chemistry, IIT Guwahati.

---

## Overview

This project applies **graph theory** to represent molecular topology and uses it to construct Hamiltonian matrices for small molecules. The Schrödinger equation is then solved numerically to obtain molecular orbital energies and electron density distributions.

**Molecules modelled:**
- H₂ (diatomic — validation case)
- H₂O
- NH₃
- CH₄
- Benzene (C₆H₆) — aromatic π-system via Hückel theory

---

## Theory

### Graph-Theoretic Representation

A molecule is represented as a weighted graph $G = (V, E)$ where:
- **Vertices** $V$ = atomic centres
- **Edges** $E$ = chemical bonds
- **Edge weights** = bond integrals (resonance integrals $\beta$)
- **Vertex weights** = Coulomb integrals $\alpha$

The **adjacency matrix** $A$ of the molecular graph directly gives the Hückel Hamiltonian:

$$H_{ij} = \begin{cases} \alpha_i & i = j \\ \beta_{ij} & \text{if } i,j \text{ bonded} \\ 0 & \text{otherwise} \end{cases}$$

### Eigenvalue Problem

Molecular orbital energies are eigenvalues of $H$:

$$H\,\mathbf{c} = E\,\mathbf{c}$$

Solved via LAPACK's `DSYEV` (symmetric eigenvalue routine) for real symmetric Hamiltonians.

### Many-Body Extension

For multi-electron systems, the many-body wavefunction is constructed as a Slater determinant of single-particle orbitals. Electron–electron repulsion is treated at the mean-field (Hartree–Fock) level.

---

## Repository Structure

```
molecular-structure-fortran/
├── src/
│   ├── graph_builder.f90        # Molecular graph construction from input
│   ├── hamiltonian.f90          # Hückel / extended-Hückel Hamiltonian assembly
│   ├── eigensolver.f90          # LAPACK DSYEV wrapper
│   ├── wavefunction.f90         # Orbital coefficients and electron density
│   ├── output.f90               # Results formatting and file output
│   └── main.f90                 # Driver program
├── molecules/
│   ├── h2.inp                   # H₂ input file
│   ├── h2o.inp                  # H₂O input file
│   ├── ch4.inp                  # CH₄ input file
│   └── benzene.inp              # Benzene input file
├── figures/
│   ├── benzene_mo_energies.png  # Molecular orbital energy diagram
│   └── electron_density.png
├── results/
│   └── sample_output.txt
├── Makefile
├── .gitignore
└── README.md
```

---

## Input Format

Each `.inp` file specifies the molecular graph:

```
# h2o.inp — water molecule
ATOMS 3
O  -0.319  0.0
H1  0.319  0.756
H2  0.319 -0.756

BONDS 2
O  H1  1.0
O  H2  1.0

PARAMETERS
alpha_O  -14.80
alpha_H  -13.60
beta     -1.75
```

---

## Build and Run

**Requirements:** gfortran ≥ 9.0 with LAPACK/BLAS

```bash
git clone https://github.com/jaideepbuksagar/molecular-structure-fortran
cd molecular-structure-fortran
make
./mol_solver molecules/benzene.inp
```

**Sample output:**
```
=== Molecular Orbital Energies (eV) ===
MO 1:  E = -15.842   (occupied)
MO 2:  E = -14.391   (occupied)
MO 3:  E = -13.205   (occupied)
...
Total energy: -127.64 eV
HOMO-LUMO gap: 4.83 eV
```

---

## Results

**Benzene π-system (Hückel theory):**

| MO | Energy (α + xβ) | Occupancy |
|---|---|---|
| 1 | α + 2β | 2 |
| 2, 3 | α + β (degenerate) | 2, 2 |
| 4, 5 | α − β (degenerate) | 0, 0 |
| 6 | α − 2β | 0 |

Predicts delocalisation energy of $2\beta$ over the Kekulé structure — consistent with published Hückel results.

---

## Why FORTRAN?

FORTRAN remains the language of choice for high-performance numerical computation in physics and chemistry — particularly for dense linear algebra operations. This project uses FORTRAN 90 with LAPACK for eigenvalue decomposition, providing a direct pipeline to production-grade scientific computing libraries.

---

## Dependencies

- gfortran ≥ 9.0 or ifort
- LAPACK and BLAS (usually available via `sudo apt install liblapack-dev libblas-dev`)

---

## License

MIT

---

## Author

**Jaideep Buksagarmath**  
B.Tech. Chemistry, IIT Guwahati  
MSc CEE, OVGU Magdeburg  
[GitHub](https://github.com/jaideepbuksagar) · [LinkedIn](https://linkedin.com/in/[handle])
