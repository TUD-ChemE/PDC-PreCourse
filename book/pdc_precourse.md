
\documentclass[12pt,a4paper]{article}
\usepackage[hmargin=2cm]{geometry}
\usepackage{amsmath}
\usepackage{float}
\usepackage{graphicx} 
\title{PDC precourse module}
\date{May 2026}

\begin{document}

\maketitle
\tableofcontents

## Exercises
This document recaps the basic process modeling skills you will need for the process dynamics and control course, and introduces some core concepts used throughout it. Specifically, you will practice:
- Setting up mass, species and energy balances for CSTRs.
- Defining state, manipulated and disturbance variables
- Solving ODE's for relevant process variables.
- Linearizing differential equations around a chosen operating point using a Taylor expansion, and introducing deviation variables.

A video for setting up CSTR balances is linked below for reference \\ 
(insert below timestamp (1:24:00) and link to the video in a nice way \\(https://collegeramavideoportal.tudelft.nl/catalogue/4052chreky/presentation/fac410a62fad4f9fa89514e50c2aa00e1d?academicYear=2022-2023-4052chrek)).
\\ (elise let me know if there's problems with the link, it should be 4052CHREK 004).

### Input and state variables
Before setting up a model, it is useful to classify the variables in a process. A process is generally described by a set of **state variables**: quantities such as a concentration, temperature, or liquid level that describe the condition of the process at any given time. The state variables change as a result of **input variables**, quantities that act on the process from outside. Depending on whether or not an input can be adjusted by the control system, we distinguish between two types:
- **Manipulated variable:** an input variable that can be actively adjusted in order to influence the process. Examples include the inlet flow rate, the coolant flow rate, or the heat input to the reactor.
- **Disturbance variable:** an input variable that affects the process but cannot be directly adjusted by the control system. Disturbances often originate outside the process and can be measured or unmeasured, and constant or time-varying. Examples include the feed concentration, the feed temperature, or the ambient temperature surrounding the reactor.

Among the state variables, the one (or combination) whose value we want to keep at, or steer towards, a desired setpoint is called the **controlled variable**. This is typically a measured process output.
\\\\

**Exercise 1 - Mass balance and species balance combined.** Consider a continuously stirred tank reactor (CSTR) with a volume $V_0$. The tank is filled up to a level that it contains a volume of $0.2V_0$ of water. At $t = 0$, a solution of salt in water with a concentration $c_0$ flows into the CSTR at a volumetric flow rate $Q$. At the same time, a valve is opened at the bottom of the tank through which fluid exits the CSTR at a rate of flow $Q/3$.
\def\labelenumi{\alph{enumi})}
\item
  Make a sketch of the problem, indicating all relevant variables.
\item
  Identify the state variables and the disturbance variables of the process.
\item
  Determine how the volume of liquid in the CSTR depends on time.
\item
  Write down the differential equation from which the salt concentration
  at the exit of the CSTR can be computed.
\item
  Use the differential equation in d) to determine how the salt
  concentration at the exit depends on time.
\item
  Suppose now that, instead of a constant outlet flow rate of $Q/3$, the outlet valve is such that the outlet flow rate depends on the liquid volume in the tank, according to
  \[
  Q_{\text{out}}(t) = k V(t), \qquad \text{with } k = \frac{5}{3}\frac{Q}{V_0},
  \]
  where $k$ has been chosen such that $Q_{\text{out}}(0) = Q/3$, consistent with the original setup. Update the differential equation for the volume from part c) to account for this.
\item
  Determine the steady state operating point $V_{ss}$: the volume at which the outlet flow rate equals the inlet flow rate $Q$. What does this operating point represent physically?
\item
  Suppose the tank has reached the operating point found in part g), when a blockage forms in the outlet pipe. As a result, the flow coefficient drops to
  \[
  k' = \frac{4}{3}\frac{Q}{V_0}.
  \]
  Determine the new steady state operating point $V_{ss}'$. Then determine how long it takes, from the moment the blockage occurs, for the volume to settle to within 5\% of this new operating point.

### Linearization of nonlinear process models
More complicated and comprehensive process models are often nonlinear. Unfortunately, nonlinear models are quite difficult to work in with directly when designing a controller for the process. To make this task easier, the nonlinear model is usually approximated as a **linear model** that is valid in the vicinity of a chosen steady state operating point. The steps for performing this linearization are shown below.

\paragraph{Step 1: Write the model as a function of states and inputs.}
Suppose a state variable $x$ evolves according to a nonlinear differential equation that depends on one or more state variables and input variables. We write the right-hand side as a function $f$:
\[
\frac{dx}{dt} = f(x_1, x_2, \dots, u_1, u_2, \dots),
\]
where $x_1, x_2, \dots$ are the state variables and $u_1, u_2, \dots$ are the input variables (manipulated and/or disturbance variables) that $f$ depends on.

\paragraph{Step 2: Determine the operating point.}
The operating point is the steady state \\ $(\bar{x}_1, \bar{x}_2, \dots, \bar{u}_1, \bar{u}_2, \dots)$ around which we want to linearize. At this point, the process is at rest, so all derivatives are zero:
\[
0 = f(\bar{x}_1, \bar{x}_2, \dots, \bar{u}_1, \bar{u}_2, \dots) \equiv f_{ss}.
\]

\paragraph{Step 3: Expand $f$ in a multivariable Taylor series.}
Close to the operating point, $f$ can be approximated by its value at steady state, plus a correction term for every variable it depends on. Each correction term is the partial derivative of $f$ with respect to that variable evaluated at the operating point, multiplied by how far that variable has moved away from its steady-state value:
\[
f \approx f_{ss} + \left(\frac{\partial f}{\partial x_1}\right)_{ss}\!\!(x_1 - \bar{x}_1) + \left(\frac{\partial f}{\partial x_2}\right)_{ss}\!\!(x_2 - \bar{x}_2) + \dots + \left(\frac{\partial f}{\partial u_1}\right)_{ss}\!\!(u_1 - \bar{u}_1) + \left(\frac{\partial f}{\partial u_2}\right)_{ss}\!\!(u_2 - \bar{u}_2) + \dots
\]
This is simply a first-order Taylor expansion: we keep only the constant and linear terms, and discard all higher-order (quadratic and above) terms, which is justified as long as the true variables stay close to the operating point.

\paragraph{Step 4: Introduce deviation variables.}
To simplify the notation, we define **deviation variables** that measure the distance of each variable from its steady-state value, denoted with a prime:
\[
x_1' = x_1 - \bar{x}_1, \qquad x_2' = x_2 - \bar{x}_2, \qquad u_1' = u_1 - \bar{u}_1, \qquad u_2' = u_2 - \bar{u}_2, \quad \dots
\]
Since $\bar{x}_1, \bar{x}_2, \dots$ are constants, we also have $\dfrac{dx}{dt} = \dfrac{dx'}{dt}$.

\paragraph{Step 5: Assemble the linearized equation.}
Using $f_{ss} = 0$ from Step 2, and substituting the deviation variables from Step 4 into the expansion from Step 3, we obtain the **linearized model**:
\[
\frac{dx'}{dt} = \left(\frac{\partial f}{\partial x_1}\right)_{ss}\!\!x_1' + \left(\frac{\partial f}{\partial x_2}\right)_{ss}\!\!x_2' + \dots + \left(\frac{\partial f}{\partial u_1}\right)_{ss}\!\!u_1' + \left(\frac{\partial f}{\partial u_2}\right)_{ss}\!\!u_2' + \dots
\]
This equation is linear in the deviation variables, with constant coefficients given by the partial derivatives evaluated at the operating point. In practice, this means: (1) write out the nonlinear differential equation, (2) find the steady-state operating point, (3) take the partial derivative of the right-hand side with respect to every state and input variable, (4) evaluate each derivative at the operating point, and (5) write the result in terms of deviation variables. Attached below is a video which discusses the errors such linear models can have as they deviate from the operating point.\\

https://www.tudelft.nl/en/eemcs/the-faculty/departments/applied-mathematics/education/prime/prime-catalogue/linearization-error-estimation-using-the-differential-1
\\\\

**Exercise 2**
A CSTR contains a catalyst that cannot flow out of the reactor. The catalyst is used to speed up the reaction $A + X \rightarrow P$. At time $t=0$, the CSTR contains the working fluid with a concentration $c_{A,0}$ of species A; the concentration of X in the reactor is initially zero. From $t=0$ onwards, a stream of the working fluid, containing both species with concentrations $c_{A,in}$ of A and $c_{X,in}$ of X, flows into the CSTR at a constant volumetric flow rate $\phi_{V,in}$. This inlet flowrate can be adjusted with a flow controller. Fluid simultaneously leaves the reactor at a volumetric flow rate $\phi_{V,out}$, such that $\phi_{V,out}=\phi_{V,in}$ at all times, so $\Delta\phi_{V,in}=\Delta\phi_{V,out}$ . The CSTR is completely full (volume $V$, can be assumed a constant) and no significant change in the density of the working fluid is observed during the entire operation. The reaction is second order: $r = k_r c_A c_X$.
\def\labelenumi{\alph{enumi})}
1. Make a sketch of the situation and draw only the relevant quantities.
1. Identify the state variables and the input variables of the process. For each input variable, indicate whether it should be considered a manipulated variable or a disturbance variable, and briefly justify your answer.
1. Set up the species mass balances for A and X. Specify the initial conditions.
1. Assume the process reaches a steady-state operating point $(\bar{c}_A, \bar{c}_X)$. Write down the algebraic equations that define these steady-state concentrations, and solve them to find explicit expressions for $\bar{c}_A$ and $\bar{c}_X$.
1. The concentration of X, $c_X$, is the controlled variable of this process: it is the quantity we ultimately want to keep at the desired setpoint. To design a controller for it, we must understand how $c_X$ responds to changes in the state and input variables. The mass balance for X is nonlinear due to the reaction term. Linearize this equation using a multi-variable Taylor series expansion around the steady-state operating point $(\bar{c}_A, \bar{c}_X, \bar{\phi}_V, \bar{c}_{A,in}, \bar{c}_{X,in})$, and introduce deviation variables (e.g.\ $c_A'(t) = c_A(t) - \bar{c}_A$) to express the result.

**Exercise 3** \\
Consider a continuously stirred tank reactor (CSTR) with a constant volume $V = 2000$ L, where a first-order, liquid-phase exothermic reaction ($A \rightarrow B$) takes place. The feed contains pure A and enters the reactor at a volumetric flow rate $q = 300$ L/min with an inlet concentration $C_{A0} = 4.0$ mol/L and a feed temperature $T_0 = 350$ K. The reactor is operated isothermally at a steady-state temperature of $\bar{T} = 350$ K, which is controlled by a cooler with an adjustable cooling duty $Q$. 

The following data are available:
- Heat capacity of the mixture, $C_p = 3.50$ J/(g K)
- Density of the mixture, $\rho = 1.15$ g/cm$^3$
- Enthalpy of reaction, $\Delta H_R = -50.00$ kJ/mol
- Reaction rate constant, $k(T) = 2.4 \cdot 10^{15} e^{-12000/T}$ min$^{-1}$ (with $T$ in Kelvin)

\def\labelenumi{\alph{enumi})}
1. Identify the state variables, the manipulated/controlled variables, and the disturbance variables of this process.
1. The conversion of component A is 95.4\%. Determine the cooling duty $Q$ (in kJ/min) required to maintain the reactor isothermally at 350K. 
1. To design a temperature controller, we must understand how the temperature responds to changes. Set up the nonlinear differential equation for the reactor temperature $T$. Then, linearize this equation using a multi-variable Taylor series expansion around the steady-state operating point to show how the rate of change of the reactor temperature depends on small deviations in **all** state and input variables.

## Solutions for process modeling
**Exercise 1**
\def\labelenumi{\alph{enumi})}
\item


\item
  
\item
  
\item
  
\item

\item
  

\item

\item


**Solutions for Exercise 2**
\def\labelenumi{\alph{enumi})}
1.  
```{figure} images/q2sketch.png
:name: fig:placeholder
Sketch of CSTR for exercise 2
```

1. 
1.   

1. 

**Solutions for Exercise 3 **
\def\labelenumi{\alph{enumi})}
1. 
**State variables:** Reactor concentration $C_A$, Reactor temperature $T$. \\
**Manipulated variable:** Cooling duty $Q$. \\
**Disturbance variables:** Feed temperature $T_0$, Feed concentration $C_{A0}$, Flow rate $q$.

1. 
Set up the steady-state energy balance to find $Q$. Let $Q$ be the heat added to the system (negative means cooling). Note that $\rho = 1.15 \text{ g/cm}^3 = 1150 \text{ g/L}$:
$$ 0 = q \rho C_p (T_0 - \bar{T}) + (-\Delta H_R) V k(\bar{T}) \bar{C}_A + Q $$
Because the feed enters at 350K and the reactor operates at 350K, the sensible heat term ($T_0 - \bar{T}$) is zero. Additionally, the reactor concentration $\bar{C}_A=(1-0.954)\times C_{CA0}=0.184$ mol/L:
$$ 0 = 0 + (50.00 \text{ kJ/mol})(2000 \text{ L})(3.09 \text{ min}^{-1})(0.184 \text{ mol/L}) + Q $$
$$ 0 = 56,876 \text{ kJ/min} + Q \implies Q = -56,876 \text{ kJ/min} $$
The reactor requires a **cooling duty** of **56,876 kJ/min**.

1. 
The nonlinear differential equation for the reactor temperature is:
$$ \frac{dT}{dt} = \frac{q}{V}(T_0 - T) + \frac{(-\Delta H_R)}{\rho C_p} k(T) C_A + \frac{Q}{\rho V C_p} $$
Let's define the right-hand side as a nonlinear function $f$ of the state variables ($T, C_A$) and input variables ($q, T_0, Q$):
$$ \frac{dT}{dt} = f(T, C_A, q, T_0, Q) $$
A first-order Taylor series expansion around the steady-state operating point $(\bar{T}, \bar{C}_A, \bar{q}, \bar{T}_0, \bar{Q})$ is:
$$ f \approx f_{ss} + \left( \frac{\partial f}{\partial T} \right)_{ss} (T - \bar{T}) + \left( \frac{\partial f}{\partial C_A} \right)_{ss} (C_A - \bar{C}_A) + $$

$$\left( \frac{\partial f}{\partial q} \right)_{ss} (q - \bar{q}) + \left( \frac{\partial f}{\partial T_0} \right)_{ss} (T_0 - \bar{T}_0) + \left( \frac{\partial f}{\partial Q} \right)_{ss} (Q - \bar{Q}) $$

By definition, at steady state, the derivative is zero, so $f_{ss} = 0$. Introducing deviation variables (e.g., $T' = T - \bar{T}$), the linearized equation becomes:
$$ \frac{dT'}{dt} = \left( \frac{\partial f}{\partial T} \right)_{ss} T' + \left( \frac{\partial f}{\partial C_A} \right)_{ss} C_A' + \left( \frac{\partial f}{\partial q} \right)_{ss} q' + \left( \frac{\partial f}{\partial T_0} \right)_{ss} T_0' + \left( \frac{\partial f}{\partial Q} \right)_{ss} Q' $$

Note that $ \frac{dT}{dt}=\frac{dT'}{dt}$. Now, we evaluate each partial derivative at the steady state.

**1. Derivative with respect to Reactor Temperature ($T$):**
$$ \frac{\partial f}{\partial T} = -\frac{q}{V} + \frac{(-\Delta H_R)}{\rho C_p} C_A \frac{\partial k(T)}{\partial T} $$
Using the chain rule on $k(T) = 2.4 \cdot 10^{15} e^{-12000/T}$:
$$ \frac{\partial k(T)}{\partial T} = k(T) \cdot \frac{12000}{T^2} $$
Evaluating at steady state ($\bar{T} = 350$ K, $\bar{C}_A = 0.185$ mol/L, $\bar{q} = 300$ L/min, $k(\bar{T}) = 3.09$ min$^{-1}$):
$$ \left( \frac{\partial k}{\partial T} \right)_{ss} = 3.09 \cdot \frac{12000}{350^2} = 0.3027 \text{ min}^{-1} \text{K}^{-1} $$
$$ \left( \frac{\partial f}{\partial T} \right)_{ss} = -\frac{300}{2000} + \frac{50.00}{(1.15)(3.50)} (0.185) (0.3027) = -0.15 + 0.695 = 0.545 \text{ min}^{-1} $$

**2. Derivative with respect to Concentration ($C_A$):**
$$ \left( \frac{\partial f}{\partial C_A} \right)_{ss} = \frac{(-\Delta H_R)}{\rho C_p} k(\bar{T}) = \frac{50.00}{(1.15)(3.50)} (3.09) = 38.38 \text{ K L mol}^{-1} \text{min}^{-1} $$

**3. Derivative with respect to Flow Rate ($q$):**
$$ \left( \frac{\partial f}{\partial q} \right)_{ss} = \frac{1}{V}(\bar{T}_0 - \bar{T}) = \frac{1}{2000}(350 - 350) = 0 $$

**4. Derivative with respect to Feed Temperature ($T_0$):**
$$ \left( \frac{\partial f}{\partial T_0} \right)_{ss} = \frac{\bar{q}}{V} = \frac{300}{2000} = 0.15 \text{ min}^{-1} $$

**5. Derivative with respect to Heat Duty ($Q$):**
$$ \left( \frac{\partial f}{\partial Q} \right)_{ss} = \frac{1}{\rho V C_p} = \frac{1}{(1150 \text{ g/L})(2000 \text{ L})(3.50 \text{ J/(g K)})} = 1.24 \cdot 10^{-7} \text{ K/J} $$

**Final Linearized Equation:**
$$ \boxed{\frac{dT'}{dt} = 0.545 T' + 38.38 C_A' + 0.15 T_0' + (1.24 \cdot 10^{-4}) Q'_{kJ}} $$

\end{document}
