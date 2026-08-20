# NACA 0012 Airfoil: 2D RANS Simulation in ANSYS Fluent

Steady-state CFD analysis of flow over a NACA 0012 airfoil at Re = 3×10⁶,
α = 8°, with lift and drag coefficients validated against published experimental data.

---

## Base Case

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

Angle of attack is introduced through the inlet velocity components
(*U_x* = *U* cos α, *U_y* = *U* sin α) as opposed to rotating the geometry in Solidworks.

**Boundary conditions**

- Velocity inlet: left, upper and lower domain boundaries
- Pressure outlet: right boundary
- No-slip wall: airfoil profile

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

### Changing Flow Domain to 50c x 50c to Observe Dependence on Domain

| Coefficient | Simulated | Experimental (Re = 3×10⁶) |
|---|---|---|
| C_l | 0.8934 | ≈ 0.85–0.90 | 
| C_d | 0.0192 | ≈ 0.012 | 

### Refined 50c x 50c Mesh
<img width="1512" height="982" alt="Screenshot 2026-08-17 at 14 36 26" src="https://github.com/user-attachments/assets/d5377299-a983-4c6b-b52a-efb011141695" />

| Coefficient | Simulated | Experimental (Re = 3×10⁶) |
|---|---|---|
| C_l | 0.8955 | ≈ 0.85–0.90 | 
| C_d | 0.0186 | ≈ 0.012 | 

Extending the domain increased C_d from 0.0128 to 0.0186, moving further from the experimental value. The reason for this is likely that the smaller domain places the pressure outlet inside the wake, thereby raising base pressure and suppressing pressure drag. This suppression partly offset the over-prediction inherent in the base case, so the closer agreement was coincidental. 


## Velocity Contours

<img width="571" height="496" alt="Velocity contours in CFD post" src="https://github.com/user-attachments/assets/15dcec63-253b-4f03-9a65-fe44c4f576cf" />

## Turbulent Kinetic Energy Contours

<img width="571" height="496" alt="Turbulent KE contours in CFD post" src="https://github.com/user-attachments/assets/809f0b2a-bef7-4361-b565-714426f8e3ea" />
## Streamlines

<img width="571" height="496" alt="Streamlines" src="https://github.com/user-attachments/assets/4ea9a6bf-3c1e-4587-99e5-7675e5aa97a8" />
## Pressure Contours

<img width="571" height="496" alt="Pressure contours" src="https://github.com/user-attachments/assets/99b0f918-5a10-4c27-b4b5-2198d93da735" />



Peak velocity of ≈ 84 m/s over the upper surface, ≈ twice freestream, coinciding
with the low-pressure region inducing the suction peak.
Turbulent kinetic energy is confined to a thin boundary layer and a
narrow wake.

---

### Changing Angle of Attack

In this section, I re-ran the simulation, altering the angle of attack to 6 degrees and 10 degrees and I obtained the following results:

| alpha | C_l | C_d |
|---|---|---|
| 6 |	0.6977 | 0.0153 |
| 10 |	1.0543 | 0.0246 |

# Coefficients of Pressure
<img width="500" height="500" alt="pressure_coeffs_inv" src="https://github.com/user-attachments/assets/6bda320f-f15f-430a-88d3-935485aaab14" />

The suction peak deepens from Cp ≈ −2.5 at 6° to −3.8 at 8° and −4.8 at 10°,
and moves forward toward the leading edge as incidence increases. All three
curves converge to Cp ≈ +0.1 at the trailing edge, consistent with the Kutta
condition being satisfied at each angle.

The area enclosed between the upper and lower surface curves is proportional
to the lift, and grows with the angle of incidence in line with the computed C_l.




## Discussion

#Drag and Lift

Predicted lift coefficient agrees well with published section data throughout. At 8° the computed C_l = 0.8955 falls within the experimental band of approximately 0.85–0.90 for this section at Re = 3×10⁶.
Drag is over-predicted by approximately 40% against experiment, and this is likely due to the fact that the simulation applies the SST k-ω model without a transition model and is therefore fully turbulent from the leading edge.
That assumption is the primary source of the discrepancy. At Re = 3×10⁶ the physical boundary layer remains laminar over a significant portion of the forward chord, particularly on the pressure surface. Laminar skin friction is significantly lower than turbulent, so imposing turbulent behaviour from the stagnation point inflates the friction drag component by a non-negligible amount.
Lift is far less affected because, at moderate incidence, it is set primarily by the outer pressure distribution, which is comparatively insensitive to boundary-layer detail.


Mesh sensitivity

Two meshes differing only in refinement level were compared on the 50c × 50c domain at 8°. C_l changed by 0.2% and C_d by 3%, indicating that the solution is approaching mesh independence.


---






## Tools

Solidworks, ANSYS Fluent 2026 R1, ANSYS Meshing, CFD-Post Processing

## References

Abbott, I.H. and von Doenhoff, A.E. (1959). *Theory of Wing Sections*. 


