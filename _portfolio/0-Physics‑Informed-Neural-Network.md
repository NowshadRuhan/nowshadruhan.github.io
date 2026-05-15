---
title: "Physics‑Informed Neural Network for Single Particle Model SOC and SOH Prediction"
excerpt: "A Physics‑Informed Neural Network (PINN) is a neural network trained not only on data but also on physical laws, typically expressed as differential equations.<br/><img src='/images/pinn.svg'>"
collection: portfolio
---

<h3> Physics‑Informed Neural Network (PINN)</h3>
<hr>

<img src='/images/pinn.svg' style="width:1150px;">

A Physics‑Informed Neural Network (PINN) is a neural network trained not only on data but also on physical laws, typically expressed as differential equations.

**_Instead of learning purely from data, the model is constrained by:_**

- Electrochemical battery physics (e.g., diffusion, degradation)
- Governing PDEs/ODEs (e.g., Fick’s law, SEI growth models)
- Boundary & initial conditions
- Conservation laws

This hybrid approach improves generalization, especially when data is sparse or noisy.
This is strongly supported by recent research showing PINNs outperform purely data‑driven models in SOH/SOC estimation under limited data conditions

<h3> Physics Relevant to Lithium‑Ion Battery Degradation </h3>
<hr>

PINNs embed physics from electrochemical models.

<h4>Single Particle Model (SPM)</h4>

<p><strong>Fick's diffusion in solid phase:</strong></p>
<p>
∂c<sub>s</sub>(r,t)/∂t =
(D<sub>s</sub> / r<sup>2</sup>) · ∂/∂r ( r<sup>2</sup> · ∂c<sub>s</sub>(r,t)/∂r ),
&nbsp;&nbsp;0 &lt; r &lt; R
</p>

<p><strong>Boundary conditions:</strong></p>
<p>
At r = 0: &nbsp; ∂c<sub>s</sub>/∂r = 0
<br/>
At r = R: &nbsp; -D<sub>s</sub> · ∂c<sub>s</sub>/∂r = j(t)/F
</p>

<p><strong>Initial condition:</strong></p>
<p>
c<sub>s</sub>(r,0) = c<sub>s,0</sub>(r)
</p>

<p><strong>Terminal voltage (simplified SPM):</strong></p>
<p>
V(t) = U<sub>p</sub>(c<sub>s,p</sub><sup>surf</sup>(t))
&minus; U<sub>n</sub>(c<sub>s,n</sub><sup>surf</sup>(t))
&minus; I(t) · R<sub>tot</sub>
</p>

**SEI Growth & Capacity Fade**

SOH degradation is often modeled:

- SEI layer growth

- Loss of active lithium

- Increase in internal resistance

<h3> Neural Network Model Design </h3>
<hr>

### Designing a PINN for SOH/SOC Prediction After 800 Cycles

_Define the Inputs and Outputs_

1. **Inputs**
   - Cycle index (0–100)
   - Charge/discharge current profile
   - Voltage curves
   - Temperature
   - Feature Engineering: Time‑series features (dV/dt, dQ/dV, etc.)

2. **Outputs**
   - SOC(t) — continuous prediction over cycle
   - SOH(cycle) — capacity fade trajectory

### Network Architecture

**PINN Architecture**

- Fully connected MLP
- 4–8 hidden layers
- 64–256 neurons per layer
- Activation: tanh (Because PDE residuals require smooth derivatives; tanh is differentiable and stable.)
- Output layer: linear

**Automatic Differentiation**

<p><strong>1. First-order time derivative:</strong></p>
<p>
∂c<sub>s</sub>(r,t) / ∂t
= 
d c<sub>s</sub>(r,t) / d t
</p>

<p><strong>2. First-order spatial derivative:</strong></p>
<p>
∂c<sub>s</sub>(r,t) / ∂r
=
d c<sub>s</sub>(r,t) / d r
</p>

<p><strong>3. Second-order spatial derivative:</strong></p>
<p>
∂<sup>2</sup>c<sub>s</sub>(r,t) / ∂r<sup>2</sup>
=
d<sup>2</sup> c<sub>s</sub>(r,t) / d r<sup>2</sup>
</p>

<p><strong>4. Full diffusion PDE residual (PINN form):</strong></p>
<p>
R<sub>diff</sub> =
∂c<sub>s</sub>/∂t
−
(D<sub>s</sub> / r<sup>2</sup>)
·
∂/∂r
(
r<sup>2</sup> · ∂c<sub>s</sub>/∂r
)
</p>

<p><strong>5. SEI growth ODE residual:</strong></p>
<p>
R<sub>SEI</sub> =
dL<sub>SEI</sub>/dt
−
(k<sub>SEI</sub> / L<sub>SEI</sub>)
</p>

<p><strong>6. Voltage residual:</strong></p>
<p>
R<sub>V</sub> =
V<sub>pred</sub>(t)
−
V<sub>measured</sub>(t)
</p>

<h3> Training Strategy </h3>
<hr>

**Sampling Strategy**

- Data points (from battery dataset)

- Collocation points (random points where PDE must hold)

- Boundary points (for BC/IC constraints)

**Optimizers**

Two‑stage training is standard:

- Adam (fast convergence)

- L‑BFGS (fine‑tuning, improves PDE satisfaction)

<h3> Loss Function Design </h3>
<hr>

This PINNs use a composite loss

**Total Loss:**

<p>
L = L<sub>data</sub> + &lambda;<sub>phys</sub> L<sub>physics</sub> + &lambda;<sub>bc</sub> L<sub>BC</sub>
</p>

**Data Loss:**

<p>
L<sub>data</sub> =
|| SOH - SOH<sub>true</sub> ||<sup>2</sup>
+ 
|| SOC - SOC<sub>true</sub> ||<sup>2</sup>
</p>

**Physics Loss:**

Enforces PDE/ODE constraints:

- Fick’s diffusion equation residual
- SEI growth ODE residual
- Voltage equation residual

**Boundary/Initial Condition Loss**

Ensures physical consistency:

- SOC ∈ [0,1]
- Concentration boundary conditions at particle surface
- Voltage limits (2.5–4.2 V)
