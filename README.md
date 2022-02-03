# Nano-3600-Assignment-1

Skip to content
Search or jump to…
Pull requests
Issues
Marketplace
Explore
 
@rcahalan01 
CambridgeUniversityPress
/
NumericalMethodsPhysicsWithPython
Public
Code
Issues
Pull requests
Actions
Projects
Wiki
Security
Insights
NumericalMethodsPhysicsWithPython/codes/action.py /
@agezerlis
agezerlis Adding codes and tutorials
Latest commit 9dafdd3 on Jul 15, 2020
 History
 1 contributor
37 lines (31 sloc)  1.06 KB
   
# Author: Alex Gezerlis
# Numerical Methods in Physics with Python (CUP, 2020)

from multi_newton import multi_newton
import numpy as np

def params():
    nvar = 100; m = 1.
    xini, xfin = 2., 0.
    tt = 1.; dt = tt/(nvar+1)
    return nvar, m, xini, xfin, dt

def fod(der,x):
    return -x**3 if der==0 else -3*x**2

def actfs(xs):
    nvar, m, xini, xfin, dt = params()
    arr = np.zeros(nvar)
    arr[0] = (m/dt)*(2*xs[0]-xini-xs[1]) + dt*fod(0,xs[0])
    arr[1:-1] = (m/dt)*(2*xs[1:-1] - xs[:-2] - xs[2:]) 
    arr[1:-1] += dt*fod(0,xs[1:-1])
    arr[-1] = (m/dt)*(2*xs[-1]-xs[-2]-xfin) + dt*fod(0,xs[-1])
    return arr

def actjac(actfs,xs):
    nvar, m, xini, xfin, dt = params()
    Jf = np.zeros((nvar,nvar))
    np.fill_diagonal(Jf, 2*m/dt + fod(1,xs)*dt)
    np.fill_diagonal(Jf[1:,:], -m/dt)   
    np.fill_diagonal(Jf[:,1:], -m/dt)   
    actfs_xs = actfs(xs)
    return Jf, actfs_xs

if __name__ == '__main__':
    nvar, m, xini, xfin, dt = params()
    xolds = np.array([2-0.02*i for i in range(1,nvar+1)])
    xnews = multi_newton(actfs, actjac, xolds); print(xnews)
© 2022 GitHub, Inc.
Terms
Privacy
Security
Status
Docs
Contact GitHub
Pricing
API
Training
Blog
About
Loading complete
