# Mixed boundary value problem for the Helmholtz equation: FreeFEM code

This repository contains the FreeFEM [1] script used for the numerical results in the manuscript on the Helmholtz equation with mixed boundary conditions by Besiana Çobani and Eriselda Goga.

## Problem

On the unit disc $\Omega$, the script computes the scattered field $phi$ that satisfies

$$
\Delta u + k^2 u = 0 \quad \text{in } \Omega,
$$

with a Dirichlet condition on Part 1 of the boundary and an impedance (Robin) condition on Part 2:

$$
varphi = f \quad \text{on } \Gamma_1 = \{\theta \in [0, 2\pi/3]\}, \qquad
\frac{\partial varphi}{\partial n} + i\alpha varphi = h \quad \text{on } \Gamma_2 = \{\theta \in [2\pi/3, 2\pi]\}.
$$

The incident plane wave is $u_{inc} = e^{ikx}$. The data are $f = -u_{inc}$ on $\Gamma_1$ and $h = 0$ on $\Gamma_2$. The total field is $varphi + u_{inc}$.

## Method

The Dirichlet data are removed with a lifting function $\varphi_0$:

1. $\varphi_0$ solves the Laplace equation in $\Omega$ with $\varphi_0 = f$ on $\Gamma_1$ and $\partial\varphi_0/\partial n = 0$ on $\Gamma_2$ (natural condition).
2. The function $w = u - \varphi_0$ satisfies a Robin condition on $\Gamma_2$ with data $\tilde{h} = h - i\alpha\varphi_0$, and $w = 0$ on $\Gamma_1$.
3. Both problems are solved with P1 finite elements, and $u = w + \varphi_0$.

## File

| File | Content |
|---|---|
| `script.edp` | Mesh, lifting, finite element solution, L2 norm, plots and boundary magnitude (Figure 6). |

## Usage

```
FreeFem++ script.edp
```

The executable keeps its old name, `FreeFem++`. The plot windows use `wait=true`, so press a key in each window to continue.

Output files:

- `Th.eps`: the mesh
- `Improved Magnitude Plot.eps`: magnitude of $u$
- `RealpartPlot.eps`: real part of the total field
- `boundary_magnitude.txt`: two columns, $\theta$ and $|u|$ on the boundary (3601 points), used for Figure 6
- `Figure6_boundary.eps`: plot of the boundary magnitude

The script prints the number of elements and the L2 norm $\|u\|_{L^2(\Omega)}$ in the console.

## Parameters

The values set in the script are $k = 7.85$, $\alpha = 2.0$ and the mesh `Part1(400) + Part2(350)`. To reproduce the other results of the paper, change the following lines.

| Result | $k$ | $\alpha$ |
|---|---|---|
| Figure 3 | 7.85 | 10 and 0.5 (magnitude) |
| Figure 4 | 15 and 3 | 2 (real part of the total field) |
| Figure 6 and Table 1 | 7.85 | 2.0 |

For Table 1, change the number of boundary nodes in `buildmesh(Part1(...) + Part2(...))`.

| Mesh | Part1 + Part2 | Elements | $h$ |
|---|---|---|---|
| I | 75 + 50 | 1,497 | 0.04579 |
| II | 150 + 100 | 5,858 | 0.02316 |
| III | 200 + 150 | 12,374 | 0.01593 |
| IV | 400 + 350 | 62,216 | 0.00711 |

Here $h = \sqrt{|\Omega_h| / N_T}$, where $N_T$ is the number of elements.

## Results (Table 1)

L2 norm of $u$ for $k = 7.85$ and $\alpha = 2.0$:

| Mesh | L2 norm | Relative difference (%) |
|---|---|---|
| I | 1.5048 | - |
| II | 1.4940 | 0.72 |
| III | 1.4760 | 1.22 |
| IV | 1.4619 | 0.96 |

The grid convergence index (GCI) [2] is computed from the three finest meshes (II, III and IV): $r_{21} = 2.2405$, $r_{32} = 1.4539$, observed order $p = 1.83$, extrapolated value $1.4577$. The finest mesh is 0.28% above the extrapolated value, with $e_a = 0.964\%$ and GCI = 0.36%.

## Requirements

FreeFEM (results obtained with FreeFEM-cs 14.2). Element counts may differ slightly between versions, because the mesh generator can change.

## Contact

Eriselda Goga, GitHub: [eriseldagoga](https://github.com/eriseldagoga)

## References

[1] Hecht, F. (2012). New development in FreeFem++. Journal of Numerical Mathematics, 20(3-4), 251-266.

[2] Celik, I. B., Ghia, U., Roache, P. J., Freitas, C. J., Coleman, H., & Raad, P. E. (2008). Procedure for estimation and reporting of uncertainty due to discretization in CFD applications. Journal of Fluids Engineering, 130(7), 078001.
