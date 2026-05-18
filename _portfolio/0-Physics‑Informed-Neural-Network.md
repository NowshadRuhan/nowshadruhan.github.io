---
title: "Physics‑Informed Neural Network for Single Particle Model SOC and SOH Prediction"
excerpt: "A Physics‑Informed Neural Network (PINN) is a neural network trained not only on data but also on physical laws, typically expressed as differential equations.<br/><img src='/images/pinn.svg'>"
collection: portfolio
---

## Physics‑Informed Neural Network (PINN)

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

## Physics Relevant to Lithium‑Ion Battery Degradation

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

## Neural Network Model Design

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

## Training Strategy

<hr>

**Sampling Strategy**

- Data points (from battery dataset)

- Collocation points (random points where PDE must hold)

- Boundary points (for BC/IC constraints)

**Optimizers**

Two‑stage training is standard:

- Adam (fast convergence)

- L‑BFGS (fine‑tuning, improves PDE satisfaction)

## Loss Function Design

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

## How PINNs Improve SOH/SOC Prediction After 800 Cycles

1. Better Generalization

PINNs can predict SOH/SOC even when:

- Voltage curves are missing

- Sensor data is sparse

- Cycles beyond training range (e.g., extrapolating to 700)

This is project demonstrated in recent works' PINNs outperform data‑driven models under sparse sensor conditions.

2. Physical Interpretability

The model learns degradation consistent with:

- SEI growth

- Lithium loss

- Diffusion limits

3. Stability

- PINNs avoid unphysical predictions (e.g., SOC > 100%).

Below (Design Pattern) code based of PINNs:

```
import torch
import torch.nn as nn
import torch.autograd as autograd

# =========================
# 1. Base MLP for PINN
# =========================

class MLP(nn.Module):
    def __init__(self, in_dim, out_dim, hidden_dim=128, n_layers=6):
        super().__init__()
        layers = []
        layers.append(nn.Linear(in_dim, hidden_dim))
        layers.append(nn.Tanh())
        for _ in range(n_layers - 1):
            layers.append(nn.Linear(hidden_dim, hidden_dim))
            layers.append(nn.Tanh())
        layers.append(nn.Linear(hidden_dim, out_dim))
        self.net = nn.Sequential(*layers)

    def forward(self, x):
        return self.net(x)


# ==========================================
# 2. PINN for battery SPM + SOH/SOC
# ==========================================

class BatteryPINN(nn.Module):
    """
    Inputs:
        x = [t, r, I, T, cycle]  (you can choose subset)
    Outputs:
        c_s(r,t)  : solid concentration
        soh(t)    : state of health
        soc(t)    : state of charge
    """
    def __init__(self, in_dim=3, hidden_dim=128, n_layers=6):
        super().__init__()
        # Example: input = [t, r, I]  -> in_dim=3
        # output = [c_s, soh, soc]   -> out_dim=3
        self.model = MLP(in_dim=in_dim, out_dim=3,
                         hidden_dim=hidden_dim, n_layers=n_layers)

        # Physical parameters (those values need to set based on measurement)
        self.D_s = 1.0e-14      # diffusion coefficient
        self.R_p = 1.0e-6       # particle radius
        self.F = 96485.3329     # Faraday constant
        self.a = 1.0e5          # specific surface area
        self.L = 1.0e-4         # electrode thickness

    def forward(self, t, r, I):
        """
        t, r, I: tensors of shape (N, 1)
        """
        x = torch.cat([t, r, I], dim=1)
        out = self.model(x)
        c_s = out[:, 0:1]
        soh = out[:, 1:2]
        soc = out[:, 2:3]
        return c_s, soh, soc

    # -----------------------------
    # Automatic differentiation
    # -----------------------------
    def gradients(self, y, x):
        """dy/dx for scalar y and vector x."""
        return autograd.grad(
            y, x,
            grad_outputs=torch.ones_like(y),
            create_graph=True,
            retain_graph=True
        )[0]

    # ======================================
    # 3. Physics residuals for PINN loss
    # ======================================

    def diffusion_residual(self, t, r, I):
        """
        Fick's diffusion PDE in spherical coordinates:
        ∂c_s/∂t - D_s/r^2 * ∂/∂r ( r^2 ∂c_s/∂r ) = 0
        """
        t.requires_grad_(True)
        r.requires_grad_(True)

        c_s, _, _ = self.forward(t, r, I)

        # First derivatives
        dc_dt = self.gradients(c_s, t)          # ∂c_s/∂t
        dc_dr = self.gradients(c_s, r)          # ∂c_s/∂r

        # Second derivative term in spherical coordinates:
        # (1/r^2) * d/dr ( r^2 * dc/dr )
        r2_dc_dr = r**2 * dc_dr
        d_r2_dc_dr_dr = self.gradients(r2_dc_dr, r)
        diffusion_term = self.D_s / (r**2 + 1e-12) * d_r2_dc_dr_dr

        R_diff = dc_dt - diffusion_term
        return R_diff

    def sei_residual(self, t, L_sei_pred):
        """
        Example SEI growth ODE:
        dL_SEI/dt - k_SEI / L_SEI = 0
        Here L_sei_pred is a NN output or a function of SOH.
        """
        k_sei = 1.0e-12  # example
        t.requires_grad_(True)
        dLdt = self.gradients(L_sei_pred, t)
        R_sei = dLdt - k_sei / (L_sei_pred + 1e-12)
        return R_sei

    def bc_residual_center(self, t, I):
        """
        Boundary condition at r = 0:
        ∂c_s/∂r |_{r=0} = 0
        """
        r0 = torch.zeros_like(t)
        r0.requires_grad_(True)
        c_s, _, _ = self.forward(t, r0, I)
        dc_dr_center = self.gradients(c_s, r0)
        return dc_dr_center  # should be ~0

    def bc_residual_surface(self, t, I):
        """
        Boundary condition at r = R_p:
        -D_s * ∂c_s/∂r |_{r=R_p} = I / (F a L)
        """
        rR = torch.ones_like(t) * self.R_p
        rR.requires_grad_(True)
        c_s, _, _ = self.forward(t, rR, I)
        dc_dr_surface = self.gradients(c_s, rR)

        flux_left = -self.D_s * dc_dr_surface
        flux_right = I / (self.F * self.a * self.L + 1e-12)
        R_bc_surface = flux_left - flux_right
        return R_bc_surface

    def ic_residual(self, t0, r, I, c_s_init):
        """
        Initial condition:
        c_s(r, 0) = c_s_init(r)
        """
        t0_ = torch.zeros_like(t0)
        c_s_pred, _, _ = self.forward(t0_, r, I)
        R_ic = c_s_pred - c_s_init
        return R_ic

    def conservation_mass_residual(self, t, r_grid, I):
        """
        Mass conservation (simplified discrete version):
        d/dt ∫ c_s(r,t) 4π r^2 dr + 4π R_p^2 j(t) = 0
        Here we approximate the integral over r_grid.
        """
        # r_grid: (N_r, 1), t: (1,1) or (N_r,1) broadcastable
        t_expanded = t.expand_as(r_grid)
        c_s, _, _ = self.forward(t_expanded, r_grid, I.expand_as(r_grid))

        # approximate integral with simple Riemann sum
        # assume uniform spacing in r
        dr = (r_grid[1] - r_grid[0]).abs()
        integrand = 4.0 * torch.pi * (r_grid**2) * c_s
        N_s = torch.sum(integrand * dr)

        # dN_s/dt via autograd
        t_single = t.clone().requires_grad_(True)
        t_expanded2 = t_single.expand_as(r_grid)
        c_s2, _, _ = self.forward(t_expanded2, r_grid, I.expand_as(r_grid))
        integrand2 = 4.0 * torch.pi * (r_grid**2) * c_s2
        N_s2 = torch.sum(integrand2 * dr)
        dNs_dt = self.gradients(N_s2, t_single)

        # surface flux term: 4π R_p^2 j(t) = 4π R_p^2 * I / (F a L)
        j = I / (self.F * self.a * self.L + 1e-12)
        flux_term = 4.0 * torch.pi * (self.R_p**2) * j

        R_mass = dNs_dt + flux_term
        return R_mass


# ==========================================
# 4. Example: building loss function
# ==========================================

def pinn_loss(model,
              t_coll, r_coll, I_coll,      # collocation points for PDE
              t_bc, I_bc,                  # BC points
              t_ic, r_ic, I_ic, c_s_init,  # IC points
              t_data, r_data, I_data,      # data points
              c_s_data, soh_data, soc_data,
              r_grid_for_mass):
    """
    Combine data loss + physics loss.
    """
    # ----- Data loss -----
    c_s_pred, soh_pred, soc_pred = model(t_data, r_data, I_data)
    loss_data = torch.mean((c_s_pred - c_s_data)**2) \
              + torch.mean((soh_pred - soh_data)**2) \
              + torch.mean((soc_pred - soc_data)**2)

    # ----- PDE residual (diffusion) -----
    R_diff = model.diffusion_residual(t_coll, r_coll, I_coll)
    loss_pde = torch.mean(R_diff**2)

    # ----- BC residuals -----
    R_bc_center = model.bc_residual_center(t_bc, I_bc)
    R_bc_surface = model.bc_residual_surface(t_bc, I_bc)
    loss_bc = torch.mean(R_bc_center**2) + torch.mean(R_bc_surface**2)

    # ----- IC residual -----
    R_ic = model.ic_residual(t_ic, r_ic, I_ic, c_s_init)
    loss_ic = torch.mean(R_ic**2)

    # ----- Mass conservation residual -----
    R_mass = model.conservation_mass_residual(t_coll[:1], r_grid_for_mass, I_coll[:1])
    loss_mass = torch.mean(R_mass**2)

    # Weights (tune these)
    λ_pde = 1.0
    λ_bc = 1.0
    λ_ic = 1.0
    λ_mass = 0.1

    loss_phys = λ_pde * loss_pde + λ_bc * loss_bc + λ_ic * loss_ic + λ_mass * loss_mass
    loss_total = loss_data + loss_phys

    return loss_total, {
        "loss_data": loss_data.item(),
        "loss_pde": loss_pde.item(),
        "loss_bc": loss_bc.item(),
        "loss_ic": loss_ic.item(),
        "loss_mass": loss_mass.item()
    }


# ==========================================
# 5. Sketch of training loop
# ==========================================

if __name__ == "__main__":
    device = "cuda" if torch.cuda.is_available() else "cpu"

    model = BatteryPINN(in_dim=3, hidden_dim=128, n_layers=6).to(device)
    optimizer = torch.optim.Adam(model.parameters(), lr=1e-3)

    # TODO: build your tensors:
    # t_coll, r_coll, I_coll, ...
    # t_bc, I_bc, ...
    # t_ic, r_ic, I_ic, c_s_init, ...
    # t_data, r_data, I_data, c_s_data, soh_data, soc_data, ...
    # r_grid_for_mass, ...

    for epoch in range(10000):
        optimizer.zero_grad()
        loss, logs = pinn_loss(
            model,
            t_coll, r_coll, I_coll,
            t_bc, I_bc,
            t_ic, r_ic, I_ic, c_s_init,
            t_data, r_data, I_data,
            c_s_data, soh_data, soc_data,
            r_grid_for_mass
        )
        loss.backward()
        optimizer.step()

        if epoch % 500 == 0:
            print(f"Epoch {epoch} | total={loss.item():.4e} | "
                  f"data={logs['loss_data']:.4e} | pde={logs['loss_pde']:.4e} | "
                  f"bc={logs['loss_bc']:.4e} | ic={logs['loss_ic']:.4e} | "
                  f"mass={logs['loss_mass']:.4e}")

```

**Note: Remember this code version is not final code version that used in production/paper**
