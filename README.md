# Exoplanet-Atmosphere

Here, I have partially reproduced the results from the research paper "ANALYTIC MODELS FOR ALBEDOS, PHASE CURVES, AND POLARIZATION OF REFLECTED LIGHT FROM EXOPLANETS" on my own, and I will explain my implementation below.

What I have implemented:

My main goal was to generate the plot of Geometric Albedo (A_g) vs. Scattering Albedo (w).
For this purpose, I implemented Lambert, Isotropic, anisotropic, Rayleigh scalar, and Rayleigh vector scattering.
Only the Lambert model matched the results from the paper. The results for Isotropic and anisotropic scattering did not match.
In the last two cases, I attempted to implement them but encountered overflow and convergence issues.
I have a question about the equation for H (Equation 16). Is the characteristic function equal to the scattering albedo (w), or is it some random constant?

If it is the scattering albedo, then the value (1-2*w)^0.5 will become negative when w>0.5. I changed it to (1-w^2) for this reason.
Similarly, the value of H used in Equations 21 and 22 is not mentioned. I assumed it was H^(0).

How I have implemented:

 1. I took the equations from the paper as-is, without delving deeply into the corresponding derivations or analysis. My primary focus was on achieving the desired results.
 2. I used the equations from the paper and the 32-quadrature method for integration (with an initial condition of 1 for H).
 3. I implemented this from scratch using the basic Python modules NumPy (for calculations) and Matplotlib (for plotting).
 4. I converted the roots of Chebyshev and Legendre polynomials to planetary coordinates using Equation 12. From there, I calculated mu and mu0 (which I call u and u0 only).
    These values were then used for further calculations.

Implementation details:

 1. Lambert: This is the simplest case. I defined functions for I and J and plotted J(alpha = 0).
 2. Isotropic Scattering: I defined H_input to iteratively find the roots at specific points, which allowed me to calculate the integral for any general u. Then, I defined the H function for any general case.
 3. Anisotropic: Similar to Isotropic scattering, but with the moment of H and two functions of H, accessed via n (= 0 for the previous one, 1 for the current one).
 4. Rayleigh Scalar: H_input and H_function (general) for n = 1, 2, 3. Then, I iteratively solved the simultaneous integral equations for the next step using the current step.
 5. Rayleigh Vector: I solved the integral equation for N using an iterative approach. After that, using I = I_l + I_r, I calculated the intensity to find j and finally, the geometric albedo.

Note: There are two types of functions: one with the word "input" and one without. The functions with "input" are used to obtain the value at the quadrature points and are subsequently used to solve the equation at some general point.  
