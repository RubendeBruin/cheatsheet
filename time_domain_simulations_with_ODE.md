Time-domain simulations using ODE from scipy

```python
# make a function for the time-domain simulation
#
# We're running the following time-domain integration
#
# x' = v
# v' = a = F/m
#
# where 
#
# we can solve this using scipy.integrate.ode
#
# ODE solves y'(t) = f(t,y)
# where y can be a vector
#
# y[0]  = x :  f[0] = v
# y[1]  = v :  f[1] = F/m
#
# where f is the function taking thich takes t and y as input arguments

def f(t,y):
    # input for the function
    x = y[0] # 
    v = y[1] # the
    
    F = some function of x and v
    
    return np.array([v, F/m])

r = ode(f)  # create the ODE
r.set_integrator('dopri5')  # Runge-Kutta 4(5)

# set initial conditions
y0 = [x0, v0]
r.set_initial_value(y0)  # initial values for y

# for storage
dt = 0.05
at = [0]
ys = [yo]
while True:
    
    at.append(r.t + dt)
    
    yt = r.integrate(r.t + dt)
    
    # store data
    ys.append(yt)
```
