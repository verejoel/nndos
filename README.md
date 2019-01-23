########################## AIM OF THIS PROJECT #########################

To design an artificial neural network that estimates the phonon density
of states from temperature dependent heat capacity data

############################# BASIC THEORY #############################

Specific heat due to phonons is related to phonon density of states by 
the integral transform:

   ![equation](https://latex.codecogs.com/gif.latex?C_V(T)=\int_0^\infty&space;d\omega&space;D\omegaD(\omega)K(\omega,T))

![equation](https://latex.codecogs.com/gif.latex?C_V) - heat capacity at constant volume
![equation](https://latex.codecogs.com/gif.latex?T) - temperature
![equation](https://latex.codecogs.com/gif.latex?R) - gas constant
![equation](https://latex.codecogs.com/gif.latex?\omega) - phonon frequency
![equation](https://latex.codecogs.com/gif.latex?D(\omega)) - phonon density of states (DOS)
![equation](https://latex.codecogs.com/gif.latex?K(\omega,T)) - kernel function

One first discretizes the DOS by defining 

   ![equation](https://latex.codecogs.com/gif.latex?D(n&space;\Delta\omega)\equiv&space;s_n\Delta\omega)

such that the problem is reformulated as a matrix equation:

   ![equation](https://latex.codecogs.com/gif.latex?C_V(T_i)=\sum_jK_{ij}s_j).

The kernel matrix K is given by

   ![equation](https://latex.codecogs.com/gif.latex?K_{ij}=3R\frac{y^2\exp(y)}{(1-\exp(y))^2})

with 

   ![equation](https://latex.codecogs.com/gif.latex?y=\frac{\hbar\omega_j}{k_BT_i}).

N.B. we set ![equation](https://latex.codecogs.com/gif.latex?\hbar=k_B=3R=1) for the time being.

Therefore we have an ![equation](https://latex.codecogs.com/gif.latex?(m\times1)) input vector of ![equation](https://latex.codecogs.com/gif.latex?(T_i,C_{v,i})) tuples, and
require an ![equation](https://latex.codecogs.com/gif.latex?(n\times1)) output vector of the ![equation](https://latex.codecogs.com/gif.latex?s_j) components of the DOS, ![equation](https://latex.codecogs.com/gif.latex?D(\omega)).

############################ PRE-PROCESSING ############################

Temperature regularization: typical maximum value is 300, so I just 
define ![equation](https://latex.codecogs.com/gif.latex?t_i=T_i/300) for now.

Then calculate the Euclidean distance of each tuple from origin to yield
input vector ![equation](https://latex.codecogs.com/gif.latex?X):

   ![equation](https://latex.codecogs.com/gif.latex?X_i=\sqrt{t_i^2+C_{v,i}^2})

![equation](https://latex.codecogs.com/gif.latex?X) is then an ![equation](https://latex.codecogs.com/gif.latex?(m\times1)) dimensional vector of Real numbers.

NOTE: for now I will design network to accept a fixed input vector 
length of ![equation](https://latex.codecogs.com/gif.latex?m=50) and output a fixed length vector with ![equation](https://latex.codecogs.com/gif.latex?n=50)
