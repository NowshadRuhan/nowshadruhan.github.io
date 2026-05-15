---
title: "Physics‑Informed Neural Network for Single Particle Model SOC and SOH Prediction"
excerpt: "A Physics‑Informed Neural Network (PINN) is a neural network trained not only on data but also on physical laws, typically expressed as differential equations.<br/><img src='/images/pinn.svg'>"
collection: portfolio
---

## Physics‑Informed Neural Network (PINN).

<img src='/images/pinn.svg' style="width:1150px;">

A Physics‑Informed Neural Network (PINN) is a neural network trained not only on data but also on physical laws, typically expressed as differential equations.

**_Instead of learning purely from data, the model is constrained by:_**

- Electrochemical battery physics (e.g., diffusion, degradation)
- Governing PDEs/ODEs (e.g., Fick’s law, SEI growth models)
- Boundary & initial conditions
- Conservation laws

This hybrid approach improves generalization, especially when data is sparse or noisy.
This is strongly supported by recent research showing PINNs outperform purely data‑driven models in SOH/SOC estimation under limited data conditions

### Physics Relevant to Lithium‑Ion Battery Degradation

PINNs embed physics from electrochemical models.

<h3>Single Particle Model (SPM)</h3>

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
