# NACA 0012 Airfoil: 2D RANS Simulation in ANSYS Fluent

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
in the wake, plus boundary-layer inflation from the aerofoil surface was used to refine the mesh.
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
| C_l | 0.840 | ≈ 0.85–0.90 | 1–7% |
| C_d | 0.0151 | ≈ 0.012 | ≈ +25% |

Experimental values read from the NACA 0012 section characteristics in Abbott &
von Doenhoff.

### Surface pressure distribution

<img width="700" height="500" alt="Pressure Chart" src="https://github.com/user-attachments/assets/8dfcdc9e-e796-4f78-97f7-74309a8ad462" />

Stagnation point at Cp ≈ +0.95 near the leading edge, with a suction peak of
Cp ≈ −3.6 on the upper surface immediately aft of it. The two branches converge to
Cp ≈ +0.1 at the trailing edge, consistent with the Kutta condition. 

### Flow field

<img width="700" height="500" alt="Pressure contours" src="https://github.com/user-attachments/assets/c2b638d6-6429-4bc1-a559-d725ddeed6bd" />
<img width="700" height="500" alt="Velocity contours in CFD post" src="https://github.com/user-attachments/assets/d255c770-85f2-4497-8e63-eb0ba356775e" />
<img width="700" height="500" alt="Turbulent KE contours in CFD post" src="https://github.com/user-attachments/assets/8bb4089a-219c-4f79-a8cf-5b9fe07abc18" />
<img width="700" height="500" alt="Streamlines" src="https://github.com/user-attachments/assets/b244be41-9b5b-4c9f-9ee8-72e9207e9715" />


Peak velocity of ≈ 84 m/s over the upper surface, ≈ twice freestream, coinciding
with the low-pressure region inducing the suction peak.
Turbulent kinetic energy is confined to a thin boundary layer and a
narrow wake.

---

## Discussion

Lift agreement is sufficient. Drag is over-predicted by roughly 25%, and the likely cause is
the fully-turbulent treatment. The model applies turbulent boundary-layer behaviour from
the leading edge, whereas at Re = 3×10⁶ the flow past the aerofoil is laminar over
a substantial fraction of the forward chord.

This would also explain why lift is less affected than drag. The lift is
dominated by the pressure distribution.

---

## Limitations


- **Results generated for a single mesh.** No grid refinement study was performed,
- the results are not demonstrated to be mesh-independent.
- **Fully turbulent.** Model doesn't capture laminar nature of flow around leading edge.
- **Short run.** 87 iterations is few for a steady RANS case. The convergence tolerance was set to
- default so to improve accuracy, the simulation can be rerun with a tolerance of around 1e-6.


---


---

## Tools

Solidworks, ANSYS Fluent 2026 R1, ANSYS Meshing, CFD-Post Processing

## References

Abbott, I.H. and von Doenhoff, A.E. (1959). *Theory of Wing Sections*. 


