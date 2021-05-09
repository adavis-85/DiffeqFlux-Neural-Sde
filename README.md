# DiffEqFlux Neural Sde
  
  DiffEqFlux of Julia is used to create a general function approximator.  This approximator uses multiple neural networks to approximate the differential equation paramaters of the data.  DiffEqFlux allows a user to insert a function in between layers of a neural network as long as the parameters is the same of the corresponding layer:
  ```
  fixed = FastChain((x,p)->x.^3,
          FastDense(3, 20, sigmoid),
          FastDense(20,20,sigmoid),
          FastDense(20, 3))
  ```
  Specifically for stochastic differential equaitons there is an element of randomness in the data or noise.  A second neural network is then used to approximate the parameter for the noise and the approximator is created:
  ```
  wiener = FastChain(FastDense(3, 20, sigmoid),
           FastDense(20, 3))

  neuralsde = NeuralDSDE(fixed, wiener, tspan, SOSRI(),sensealg=TrackerAdjoint(),
                       saveat = tsteps, reltol = 1e-1, abstol = 1e-1,allow_f_increases=true)
                       
 ```
 Just as in a normal neural network setup there is a loss function, optimizer and callback function needed.  With DiffEqFlux it is often represented graphically to check the approximation to the fit of the data.
 ```
 
result = DiffEqFlux.sciml_train((p) -> loss_neuralsde(p),
                                 neuralsde.p, opt,
                                 cb = call, maxiters = 500)
```


  
  
