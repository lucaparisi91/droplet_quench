## Linearization of the Gross-Pitaevskii 
The GP equation is given by 
$$i\hbar\frac{\partial \phi}{\partial t}=\left( \frac{-\hbar^2}{2m}  \nabla^2 + V(r) + g|\phi|^2 \right)\phi  $$
On can look for solutions of the form 
$$ \phi(r) = e^{-i\mu t}\phi_0(r)\left( \sum_i u_i(r) e^{-i\omega_i t}  + v_i^*(r)e^{i\omega_i t}\right)$$
Looking for stationary solutions of this form on gets the equations
$$ \hbar \omega u_i = (H_0 - \mu + 2g\phi_0^2)u_i + g\phi_0^2v_i$$
$$ - \hbar \omega v_i = (H_0 - \mu + 2g\phi_0^2)v_i + g\phi_0^2u_i$$

where $H_0= \left( \frac{-\hbar^2}{2m}  \nabla^2 + V(r) \right)$ .
The functions $u_i(r)$ and $v_i(r)$ satisfy the relationships
$$\int u_iu_j^* - v_iv_j^* = \delta_{ij}$$
$$\int u_iv_j^* - v_iu_j^* = 0$$
For an uniform system solutions are plane waves $u(r)=ue^{ik\cdot r}$ and $u(r)=ue^{ik\cdot r}$ and using the normalizations above and $\mu=gn$ one gets the Bogoliubov spectrum
$$\hbar \omega = \sqrt{ \left( \frac{\hbar^2k^2}{2m} \right)^2 + \frac{\hbar^2k^2}{m}gn } $$ 

## Linearization of the Petrov equation
The petrov equation is given by $$i\hbar\frac{\partial \phi}{\partial t}=\left( \frac{-\hbar^2}{2m}  \nabla^2 + V(r) -3|\phi|^2 + \frac{5}{2}|\phi|^3  - \mu\right)\phi $$
By reapeating the process for the GP equation one gets
$$ \hbar \omega u_i = \left(H_0 - \mu + \frac{5}{2}\phi_0^3 - \frac{9}{4}\phi_0^2\right)u_i + \left( \frac{3}{4}\phi_0^2 \right)v_i $$
$$ -\hbar \omega v_i = \left(H_0 - \mu + \frac{5}{2}\phi_0^3 - \frac{9}{4}\phi_0^2\right)v_i + \left( \frac{3}{4}\phi_0^2 \right)u_i $$
Using $\mu = -3n +\frac{5}{2}n^{3/2}$ for an homogeneous system we get the spectrum
$$\hbar\omega=\sqrt{ \left(\frac{\hbar^2k^2}{2m}\right)^2 + \frac{\hbar^2 k^2}{m}\frac{3}{4}n} $$

## Implementation
If the hamiltonian is rotationally invariant one can works in spherical coordinates and the hamiltonian becomes $H_0=-\frac{\hbar^2}{2m}\frac{\partial^2 }{\partial^2 r} + \frac{2}{r}\frac{\partial}{\partial r} - \frac{l(l+1)}{r^2}$ with boundary conditions $\partial_r\psi_0(r)=0$ at the boundaries. Makes use of PETSC with MIRROR DM boundary ( to mimic Neumann boundary conditions ). The ground state is found using a Non linear Krylov Newton method.