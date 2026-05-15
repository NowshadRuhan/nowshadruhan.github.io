---
title: "Physics‑Informed Neural Network for Single Particle Model SOC and SOH Prediction"
excerpt: "A Physics‑Informed Neural Network (PINN) is a neural network trained not only on data but also on physical laws, typically expressed as differential equations.<br/><img src='/images/pinn.svg'>"
collection: portfolio
---

## PINN.

<img src='/images/pinn.svg'>

A Physics‑Informed Neural Network (PINN) is a neural network trained not only on data but also on physical laws, typically expressed as differential equations.

_Instead of learning purely from data, the model is constrained by:_

- Electrochemical battery physics (e.g., diffusion, degradation)
- Governing PDEs/ODEs (e.g., Fick’s law, SEI growth models)
- Boundary & initial conditions
- Conservation laws

This hybrid approach improves generalization, especially when data is sparse or noisy.
This is strongly supported by recent research showing PINNs outperform purely data‑driven models in SOH/SOC estimation under limited data conditions
