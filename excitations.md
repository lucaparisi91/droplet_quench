## Linearization of the Gross-Pitaevskii 
The GP equation is given by 
$$i\hbar\frac{\partial \phi}{\partial t}=\left( \frac{-\hbar^2}{2m}  \nabla^2 + V(r) + g|\phi|^2 \right)\phi  $$
On can look for solutions of the form 
$$ \phi(r) = e^{-i\mu t}\phi_0(r)\left( \sum_i u_i(r) e^{-i\omega_i t}  + v_i^*(r)e^{i\omega_i t}\right)$$
Looking for stationary solutions of this form on gets the equations

$$\hbar \omega 
\begin{pmatrix}
u \\ 
v
\end{pmatrix}=
\begin{pmatrix}
H_0 - \mu + 2g\phi_0^2 & g\phi_0^2 \\
-g\phi_0^2 & -(H_0 - \mu + 2g\phi_0^2) \\
\end{pmatrix}
\begin{pmatrix}
u \\ 
v
\end{pmatrix}
$$

where $H_0= \left( \frac{-\hbar^2}{2m}  \nabla^2 + V(r) \right)$ .
The functions $u_i(r)$ and $v_i(r)$ satisfy the relationships
$$\int u_iu_j^* - v_iv_j^* = \delta_{ij}$$
$$\int u_iv_j^* - v_iu_j^* = 0$$
For an uniform system solutions are plane waves $u(r)=ue^{ik\cdot r}$ and $u(r)=ue^{ik\cdot r}$ and using the normalizations above and $\mu=gn$ one gets the Bogoliubov spectrum
$$\hbar \omega = \sqrt{ \left( \frac{\hbar^2k^2}{2m} \right)^2 + \frac{\hbar^2k^2}{m}gn } $$ 
The quantity $\omega(k)$ also sets the growth rate of the condensate.
## Linearization of the Petrov equation
The petrov equation is given by $$i\hbar\frac{\partial \phi}{\partial t}=\left( \frac{-\hbar^2}{2m}  \nabla^2 + V(r) -3|\phi|^2 + \frac{5}{2}|\phi|^3  - \mu\right)\phi $$
By reapeating the process for the GP equation one gets
$$\hbar \omega u_i = \left(H_0 - \mu + \frac{5}{2}\phi_0^3 - \frac{9}{4}\phi_0^2\right)u_i + \left( \frac{3}{4}\phi_0^2 \right)v_i $$
$$-\hbar \omega v_i = \left(H_0 - \mu + \frac{5}{2}\phi_0^3 - \frac{9}{4}\phi_0^2\right)v_i + \left( \frac{3}{4}\phi_0^2 \right)u_i $$
Using $\mu = -3n +\frac{5}{2}n^{3/2}$ for an homogeneous system we get the spectrum
$$\hbar\omega=\sqrt{ \left(\frac{\hbar^2k^2}{2m}\right)^2 + \frac{\hbar^2 k^2}{m}\frac{3}{4}n} $$
There is not a maximum in the energy spectrum. The formation of droplets cannot be obtained at linear order.
## Implementation
If the hamiltonian is rotationally invariant one can works in spherical coordinates and the hamiltonian becomes $H_0=-\frac{\hbar^2}{2m}\frac{\partial^2 }{\partial^2 r} + \frac{2}{r}\frac{\partial}{\partial r} - \frac{l(l+1)}{r^2}$ with boundary conditions $\partial_r\psi_0(r)=0$ at the boundaries. Makes use of PETSC with MIRROR DM boundary ( to mimic Neumann boundary conditions ). The ground state is found using a Non linear Krylov Newton method. Updating the chemical potential along the simulations does not seem to work in spherical coordinates ( why ? , have tried different implementations yielding the same result  and tested each step ). Opted out for fixing the chemical potential insted of particle number. The ground state is found trough propagation in imaginary time follower by a non linear Newton-Krilov solver.
Can obtain very small accuracy. For $g=100,N=1000,\mu=15,R=10$ I get accuracy $||(H-\mu)\psi||<1e-9 $.
## BdG
### Harmonic oscillator 1D
Solution is sensible to space step for high energy excitations, where the function becomes very wiggly and it becomes difficult to resolve the ery fast oscillations
| ![image](BdG/energy-1d-harmonic-oscillator.png ) |
|:--:|
|relative-energy-difference: Relative energy difference between numerical and exact energy of excited state n obtained by exact diagonalization  |
| ![image](BdG/1d-ho-excitedn5.png ) |
|:--:|
|excited-state-n5: Wavefunction of harmonic oscillator with 500 discretization points, state n=5 obtained by exact diagonalization. Dashed lines are results from exact diagonalization.   |
| ![image](BdG/1d-ho-excitedn80.png ) |
|:--:|
|excited-state-n80: Wavefunction of harmonic oscillator with 500 discretization points, state n=80 obtained by exact diagonalization.   |

Similar results are obtained when considering an harmonic oscillator in 3D, getting agreements at low energy with $E_n = \hbar\omega(2n + l )$ where $l=0$
### Single component GP - Thomas Fermi Limit
in 3D the Thomas Fermi approximation yields the density profile
$$n(r)=n(0)\left(1 - V(r) \right)$$
$$n(0)=\frac{m\omega^2R^2}{2g}$$
$$R=\left( \frac{15}{4\pi} \frac{g}{m\omega^2}N\right)^{1/5}$$
 ![image]( BdG/TFgs-mu200g100.png) |
|:--:|
|TF-profile: Density for $\mu=200$ ,$g=200$ ,$n=1000$. a) GS is the numerical ground state of the GP equation , b) Thoams Fermi density profile  |

 ![image]( BdG/energy-spectrum-TF.png) |
|:--:|
|TF-profile: Excited states  for $\mu=200$ ,$g=200$ ,$n=1000$. a) GS is the numerical ground state of the GP equation , b) Thomas Fermi density profile  |

At low energy TF and numerical BdG agree for low energy excitation. High energy deviations are due in part because the TF limit is only approximate and in part because ground state is very wiggly and the resolution of the grid is not sufficient to resolve the excited states.
![image]( BdG/BdG-excitedn50.png) |
|:--:|
|TF-profile: Excited state $n=50$ for $\mu=200$ ,$g=200$ ,$n=1000$. a) $u(r)$ Bogoliubov function b) $v(r)$ Bogoliubov function.
## Dynamic structure factor
Is defined as [[F.Zambelli et al 2000]][dynamic-stringari]
$$ S(q,\omega)=\sum_n |<n| e^{iq\cdot r} |0 >  |^2 \delta(\omega- E_{n})$$ 
and can be computed once the eigen-states are known. For BdG excited states this takes the form 
$$<n|\rho_q|0>=\int (u^*(r) + v^*(r) )e^{iq\cdot r}\psi_0(r)$$ 
For an uniform system one gets
$$u_p(r)=Ue^{ip\cdot r}$$
$$u_p(r)=Ve^{ip\cdot r}$$
If the system is described by an uniform GP equation one gets 
$$<n|\rho_q|0>=\delta_{k_n ,q}(U + V)$$
with 
$$(U+V)^2=\frac{p^2}{2m\epsilon(p) }$$
Hence
$$S(q,\omega)=\frac{q^2}{2m\epsilon_q}\delta(\hbar\omega - \epsilon(q) )$$
The same formula applies for the Petrov equation in an uniform system although with a different $\epsilon(q)$. Indeed the relationship above is quite general if one assumes the energy spectrum to be a dirac distribution and neglects states in a broad continuum.
In the LDA approximation the dynamic structure factor takes the form
$$S_{LDA}=\int n(r)\delta(\hbar \omega - e(n(r),q) )\frac{q^2}{2m\epsilon(n(r),q)}$$ 
For a single component Bose gas in a trap the equations take the form
$$S(q,\omega)=\frac{15}{8}\frac{\omega^2 - E_r^2}{E_r \mu^2}\sqrt{1 - \frac{\omega^2 - E_r^2}{2E_r\mu}}$$
with
$\mu=(15N\frac{a}{a_{ho}})^{2/5}\hbar\omega_0$ , $\omega_0$ the frequency of the harmonic oscillator.
The value is non zero only between the recoil energy $E_{min}=E_r$ and  $E_{max}=E_r\sqrt{1+2\mu/E_r}$.<br>
At large momentum transfers $E_{max}-E_{min}\rightarrow \mu$ . 
```python
def plot_S(S,q,omega,mu=None):
    """
    S : array with dynamic structure values
    q: transferred momenta axis , 1D array
    omega : energy axis, 1D array
    """
    q_labels=["{:.2f}".format(_q) for _q in q]
    omega_labels=["{:.2f}".format(_omega) for _omega in omega]

    S_data=pd.DataFrame(data=S.transpose(),index=omega_labels,columns=q_labels)
    ax=sns.heatmap(S_data,xticklabels=len(q)//5,yticklabels=len(omega)//5)
    #ax=sns.heatmap(S_data)

    ax.axes.invert_yaxis()
    sns.lineplot(x=q/np.max(q)*len(q), y=0.5*q**2/np.max(omega)*len(omega),color="r" ,label="recoil energy")
    Er=q**2/2
    if mu is not None:
            sns.lineplot(x=q/np.max(q)*len(q), y=(Er * np.sqrt(1+2*mu/Er) )/np.max(omega)*len(omega),color="g" ,
                         label=r"$E_r\sqrt{1+2\mu/E_r}$")

    plt.xlabel(r"$q$")
    plt.ylabel(r"$\omega$")

```
![image](BdG/S_LDA_GP.png)
|Dynamical structure factor $S(q,\omega)$ for g=100 and N=1000 in harmonic oscillator units.

The integration can be performed numerically and one obtains similar results.
```python
from scipy.optimize import brentq
def epsilon(q,n,g):
    return np.sqrt(q**2/2*(q**2/2 + 2*g*n))
def dynamicStructureFactor( epsi, q, omega,density,r,maxN=10000):
    if omega <= epsi(q,0):
        return 0
    try:
        n=brentq(lambda n : epsi(q,n) - omega ,0,maxN)
    except ValueError:
        return 0
    if n > np.max(density):
        return 0
    i=np.argmin(np.abs(density-n))
    
    if i ==0 or i==len(r)-1:
        return 0
    
    dr=r[1] - r[0]
    depsidr=(epsi(q,density[i+1] ) - epsi(q,density[i-1 ] ))/(2*dr)
    depsidr=np.abs(depsidr)
    S=np.pi*4*r[i]**2*density[i]*q**2*0.5/omega*1/depsidr
    return( S)
```


[dynamic-stringari]: https://journals.aps.org/pra/pdf/10.1103/PhysRevA.61.063608

