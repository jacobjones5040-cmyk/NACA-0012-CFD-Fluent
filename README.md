# NACA 0012 Airfoil — 2D RANS Simulation in ANSYS Fluent

Steady-state CFD analysis of flow over a NACA 0012 airfoil at Re = 3×10⁶,
α = 8°, with lift and drag coefficients validated against published experimental data.

---

## Case setup

| Parameter | Value |
|---|---|
| Chord, *c* | 1.0 m |
| Freestream velocity, *U* | 43.8 m/s |
| Reynolds number, Re | 3×10⁶ |
| Angle of attack, α | 8° |
| Fluid | Air, ρ = 1.225 kg/m³, ν = 1.46×10⁻⁵ m²/s |
| Solver | Pressure-based, steady, 2D |
| Turbulence model | SST k-ω |
| Iterations to convergence | 87 |

Angle of attack is imposed through the inlet velocity components
(*U_x* = *U* cos α, *U_y* = *U* sin α) as opposed to rotating the geometry in Solidworks.

**Boundary conditions**

- Velocity inlet — left, upper and lower domain boundaries
- Pressure outlet — right boundary
- No-slip wall — airfoil profile

---

## Mesh

<img width="1000" height="650" alt="Screenshot 2026-08-13 at 09 59 45" src="https://github.com/user-attachments/assets/450a90e4-d21f-4292-8428-3a135bf4a810" />


Mesh was generated adaptive sizing. Sphere-of-influence around the aerofoil and
in the wake, plus boundary-layer inflation from the aerofoil surface was used to refine the mesh and 
exceed min orthogonal quality required by fluent (0.01)
- Cells: 43,740
- Orthogonal quality: min 0.0115, mean 0.939 (SD 0.088)

---

## Results

### Convergence

<img width="700" height="500" alt="Cl" src="https://github.com/user-attachments/assets/27755f89-ff71-44ed-b1da-7e258a1253c1" />


<img width="700" height="500" alt="Cd" src="https://github.com/user-attachments/assets/3d6f4b48-abc5-468c-bbef-d7df51ccd8cf" />

Both coefficients pass through a large transient over the first ~30 iterations,
Cl dips briefly negative around iteration 27, before settling. Cl is flat from roughly
iteration 60 onward; Cd from roughly iteration 50.

| Coefficient | Simulated | Experimental (Re = 3×10⁶) | Difference |
|---|---|---|---|
| C_l | 0.840 | ≈ 0.84–0.88 | 1–7% |
| C_d | 0.0151 | ≈ 0.012 | ≈ +25% |

Experimental values read from the NACA 0012 section characteristics in Abbott &
von Doenhoff. Note that drag there is presented as a polar against C_l rather than
against α, so C_d is read at the corresponding lift coefficient.

### Surface pressure distribution

![Cp distribution along the airfoil](images/Pressure_Chart.png)

Stagnation point at Cp ≈ +0.95 near the leading edge, with a suction peak of
Cp ≈ −3.6 on the upper surface immediately aft of it. The two branches converge to
Cp ≈ +0.1 at the trailing edge, consistent with the Kutta condition. The sharp
leading-edge suction peak and gradual pressure recovery are characteristic of a
symmetric section at moderate incidence, well below stall.

### Flow field

![Pressure contours](images/Pressure_contours.png)
![Velocity contours](images/Velocity_contours_in_CFD_post.png)
![Streamlines](images/Streamlines.png)
![Turbulent kinetic energy](images/Turbulent_KE_contours_in_CFD_post.png)

Peak velocity of ≈ 84 m/s over the upper surface, close to twice freestream, coinciding
with the low-pressure region driving the suction peak. Streamlines remain attached over
the full chord — expected at 8°, since the NACA 0012 stalls at roughly 16° at this
Reynolds number. Turbulent kinetic energy is confined to a thin boundary layer and a
narrow wake, with no separation bubble.

---

## Discussion

Lift agreement is good. Drag is over-predicted by roughly 25%, and the likely cause is
the fully-turbulent treatment: the model applies turbulent boundary-layer behaviour from
the leading edge, whereas at Re = 3×10⁶ the physical airfoil sustains laminar flow over
a substantial fraction of the forward chord. Since laminar skin friction is markedly
lower than turbulent, integrating a fully-turbulent boundary layer over the whole surface
inflates the friction drag component.

This also explains why lift is less affected than drag. Lift at moderate incidence is
dominated by the pressure distribution, which is set largely by the outer inviscid flow
and is comparatively insensitive to boundary-layer detail. Drag at this Reynolds number
is friction-dominated, so it inherits the error directly.

---

## Limitations

Stated plainly, since they bound what the results can be used for:

- **Single mesh.** No grid refinement study was performed, so the discretisation error
  is unquantified and the results are not demonstrated to be mesh-independent.
- **No inflation layer.** The mesh is pure triangular with no structured near-wall
  layers, so *y*⁺ is uncontrolled and the boundary layer is resolved by whatever the
  local cell size happens to give.
- **Fully turbulent.** No transition model, for the reasons above.
- **Short run.** 87 iterations is few for a steady RANS case; the force monitors are
  flat but residual levels should be confirmed before treating this as converged.
- **Single operating point.** One angle of attack, so no lift curve or drag polar.

---

## Next steps

1. Grid refinement study across three or more systematically refined meshes, tracking
   C_d and reporting a Grid Convergence Index.
2. Add a boundary-layer inflation layer targeting *y*⁺ ≈ 1, and record *y*⁺ per mesh.
3. Sweep α from −4° to 16° to produce a lift curve and drag polar, and compare the
   predicted stall angle against experiment.
4. Compare turbulence models at fixed mesh against the NASA Turbulence Modeling
   Resource reference values.
5. Test a transition-sensitive model (γ–Re_θ) to see whether the drag over-prediction
   closes.

---

## Repository contents

```
├── images/          Contours, streamlines, convergence and Cp plots
├── journals/        Fluent journal file for solver setup
├── post/            CFD-Post session file
├── data/            Exported Cp distribution and force coefficient histories
└── README.md
```

Fluent case, data and mesh files are excluded — they exceed GitHub's file size limits.
The journal file reproduces the solver setup from a meshed geometry.

---

## Tools

ANSYS Fluent 2026 R1 (Student), ANSYS Meshing, CFD-Post

## References

Abbott, I.H. and von Doenhoff, A.E. (1959). *Theory of Wing Sections*. Dover.

Ladson, C.L. (1988). *Effects of Independent Variation of Mach and Reynolds Numbers on
the Low-Speed Aerodynamic Characteristics of the NACA 0012 Airfoil Section*.
NASA TM 4074.

NASA Langley Turbulence Modeling Resource — 2D NACA 0012 Airfoil Validation Case.
https://turbmodels.larc.nasa.gov/naca0012_val.html
