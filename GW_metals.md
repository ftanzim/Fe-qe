# GW for metals

**Notes discussion with Dr F. Palear**:

for 3D systems the reference to look at [is this one 1a](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.107.155130)

The main difference with respect to a regular calculation on a gapped system is that we have to correct the dynamical screening function $\eps^-1 = 1 + v\chi$ (v :Coulomb potential, $\chi$ : Response function in RPA) $W = \eps^-1 v$. The reason is that at q=0, the intraband electronic transition $nk \rightarrow nk + q$ is finite but not captured. Instead, at q!-0, there are no problems and all interband and intraband transistions $nk\rightarrow mk+q$ are captured. The yambo databases ndb.pp and/or ndb.em1d contain the quantity $Y=v\Chi$, which is the one to be corrected as in Ref. [1a]

First of all, the plasmon-pole approximation for Y (PPA) should not be used here and we'd better stick with the full-frequency calculation (FF). However, in Ref.[1a] it is shown that the new multipole approximation (MPA) also works well at a lower computational cost.

Second, how to actually correct $Y(q,w) = v(q)\chi(q,w)$ for the intraband term at q=0? We have three possibilities.
1) Add a Drude term [medium]
Adding a Drude term to the calculation of $\eps^-1$ can be done by specifying the model parameters in the yambo input file via the variable `DrudeWXd`. This requires the plasma frequency (wp) and the damping parameter (g), since the model is $Y(w)=wp/[w(w+i*g)]$. There are a couple of options about how to obtain these parameters. First, the plasma frequency can be computed via DFT (e.g. with VASP). Another possibility is to fit an independent-particles absorption spectrum (Im{eps}) computed at low frequencies with the Drude model, in order to obtain values for both wp and g at the same time.
2) Use the 'Constant approximation' (CA) [easy]
The CA for 3D metals was done in Ref. [1a] and it uses the fact that for small q, the quantity Y=vX tends to a constant value in 3D. Therefore, the value Y(q=0,w) in the database is replaced with Y(q=q1,w), where q1 is the closest finite q-point to q=0 in the mesh used. The CA approximation seems to work well, it’s easy to use and does not require parameters. Its accuracy depends on the density of the q-mesh.
3) Extrapolation of intraband poles [hard]
In this method, also explained in Ref. [1a], one has to study the calculated Y(q,w) function and identify the intraband poles both at q=0 (which will be wrong) and at small q-points (which will be correct). The usage of a sum rule can help distinguish which poles are intraband and which are interband (explained in the text). Then, one extrapolates the real value of the q=0 intraband pole from the finite-q ones. This can be useful if we don’t trust the Drude parameters or have a mesh that is not dense enough for the CA.


All in all, our preference method here would be the (1.1b) CA rather than (1.1a) Drude or (1.1c) extrapolation, but all are viable.