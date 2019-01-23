########################### AIM OF THIS PROJECT ##########################
#
# To design an artificial neural network that estimates the phonon density
# of states from temperature dependent heat capacity data
#
############################## BASIC THEORY ##############################
#
# Specific heat due to phonons is related to phonon density of states by 
# the integral transform:
#
#     Cv(T) = int_0^infty [dw D(w) K(w,T)]
#
# Cv - heat capacity at constant volume
# T - temperature
# R - gas constant
# w - phonon frequency
# D(w) - phonon density of states (DOS)
# K(w,T) - kernel function
#
# One first discretizes the DOS by defining 
#
#     D(n dw) = s_n dw
#
# such that the problem is reformulated as a matrix equation:
#
#     Cv(T_i) = sum_j [K_ij s_j].
#
# The kernel matrix K is given by
#
#     K_ij = 3R y^2 exp(y)/(1-exp(y))^2
#
# with 
#
#     y = (hbar w_j)/(k_B T_i).
#
# N.B. we set hbar = k_B = 3R = 1 for the time being.
#
# Therefore we have an (m by 1) input vector of (T_i, Cv_i) tuples, and
# require an (n by 1) output vector of the s_j components of the DOS, D(w)
#
############################# PRE-PROCESSING #############################
#
# Temperature regularization: typical maximum value is 300, so I just 
# define t_i = T_i / 300 for now
#
# Then calculate the Euclidean distance of each tuple from origin to yield
# input vector X:
#
#     X_i = (t_i^2 + Cv_i^2)^0.5
#
# X is then a (m x 1) dimensional vector of Real numbers.
#
# NOTE: for now I will design network to accept a fixed input vector 
# length of m = 50 and output a fixed length vector with n = 50