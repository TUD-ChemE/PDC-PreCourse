# Linearization of Non-Linear Process Models

## Theory
More complicated and comprehensive process models are often nonlinear. Unfortunately, nonlinear models are quite difficult to work in with directly when designing a controller for the process. To make this task easier, the nonlinear model is usually approximated as a **linear model** that is valid in the vicinity of a chosen steady state operating point. The steps for performing this linearization are shown below.

### Step 1: Write the model as a function of states and inputs {-}
Suppose a state variable $x$ evolves according to a nonlinear differential equation that depends on one or more state variables and input variables. We write the right-hand side as a function $f$:  
$
\frac{dx}{dt} = f(x_1, x_2, \dots, u_1, u_2, \dots),
$
where $x_1, x_2, \dots$ are the state variables and $u_1, u_2, \dots$ are the input variables (manipulated and/or disturbance variables) that $f$ depends on.

### Step 2: Determine the operating point {-}
The operating point is the steady state \\ $(\bar{x}_1, \bar{x}_2, \dots, \bar{u}_1, \bar{u}_2, \dots)$ around which we want to linearize. At this point, the process is at rest, so all derivatives are zero:  
$
0 = f(\bar{x}_1, \bar{x}_2, \dots, \bar{u}_1, \bar{u}_2, \dots) \equiv f_{ss}.
$

### Step 3: Expand $f$ in a multivariable Taylor series {-}
Close to the operating point, $f$ can be approximated by its value at steady state, plus a correction term for every variable it depends on. Each correction term is the partial derivative of $f$ with respect to that variable evaluated at the operating point  ($\frac{\partial f}{\partial j}\right_{ss}$, where j is the variable of interest), multiplied by how far that variable has moved away from its steady-state value:  
$
f \approx f_{ss} + \left(\frac{\partial f}{\partial x_1}\right)_{ss}\!\!(x_1 - \bar{x}_1) + \left(\frac{\partial f}{\partial x_2}\right)_{ss}\!\!(x_2 - \bar{x}_2) + \dots + \left(\frac{\partial f}{\partial u_1}\right)_{ss}\!\!(u_1 - \bar{u}_1) + \left(\frac{\partial f}{\partial u_2}\right)_{ss}\!\!(u_2 - \bar{u}_2) + \dots
$  

This is simply a first-order Taylor expansion: we keep only the constant and linear terms, and discard all higher-order (quadratic and above) terms, which is justified as long as the true variables stay close to the operating point.

### Step 4: Introduce deviation variables {-}
To simplify the notation, we define **deviation variables** that measure the distance of each variable from its steady-state value, denoted with a prime:  
$
x_1' = x_1 - \bar{x}_1, \qquad x_2' = x_2 - \bar{x}_2, \qquad u_1' = u_1 - \bar{u}_1, \qquad u_2' = u_2 - \bar{u}_2, \quad \dots
$  

Since $\bar{x}_1, \bar{x}_2, \dots$ are constants, we also have $\dfrac{dx}{dt} = \dfrac{dx'}{dt}$.

### Step 5: Assemble the linearized equation {-}
Using $f_{ss} = 0$ from Step 2, and substituting the deviation variables from Step 4 into the expansion from Step 3, we obtain the **linearized model**:  
$
\frac{dx'}{dt} = \left(\frac{\partial f}{\partial x_1}\right)_{ss}\!\!x_1' + \left(\frac{\partial f}{\partial x_2}\right)_{ss}\!\!x_2' + \dots + \left(\frac{\partial f}{\partial u_1}\right)_{ss}\!\!u_1' + \left(\frac{\partial f}{\partial u_2}\right)_{ss}\!\!u_2' + \dots
$

This equation is linear in the deviation variables, with constant coefficients given by the partial derivatives evaluated at the operating point. In practice, this means:   
(1) write out the nonlinear differential equation  
(2) find the steady-state operating point  
(3) take the partial derivative of the right-hand side with respect to every state and input variable  
(4) evaluate each derivative at the operating point  
(5) write the result in terms of deviation variables

Attached below is a video which discusses the errors such linear models can have as they deviate from the operating point.

<iframe width="560" height="315" src="https://www.youtube.com/embed/pPbmzqgl9as?si=IJ-vD8dyP90XoATy" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Exercises

### Exercise A
A CSTR contains a catalyst that cannot flow out of the reactor. The catalyst is used to speed up the reaction $A + X \rightarrow P$. At time $t=0$, the CSTR contains the working fluid with a concentration $c_{A,0}$ of species A; the concentration of X in the reactor is initially zero. From $t=0$ onwards, a stream of the working fluid, containing both species with concentrations $c_{A,in}$ of A and $c_{X,in}$ of X, flows into the CSTR at a constant volumetric flow rate $\phi_{V,in}$. This inlet flowrate can be adjusted with a flow controller. Fluid simultaneously leaves the reactor at a volumetric flow rate $\phi_{V,out}$, such that $\phi_{V,out}=\phi_{V,in}$ at all times, so $\Delta\phi_{V,in}=\Delta\phi_{V,out}$.  
The CSTR is completely full (volume $V$, can be assumed a constant) and no significant change in the density of the working fluid is observed during the entire operation. The reaction is second order: $r = k_r c_A c_X$.  

```{exercise}
:label: ExerciseLin1
Make a sketch of the situation and draw only the relevant quantities.
```
`````{solution} ExerciseLin1
:label: SolutionLin1
:class: dropdown
```{figure} images/solutions/q2sketch.png
:name: fig:placeholder
Sketch of CSTR.
```
`````

```{exercise}
:label: ExerciseLin2
Identify the state variables and the input variables of the process. 

For each input variable, indicate whether it should be considered a manipulated variable or a disturbance variable, and briefly justify your answer.
```
```{solution} ExerciseLin2
:label: SolutionLin2
:class: dropdown
The **state variables** are the concentrations inside the reactor, $c_A(t)$ and $c_X(t)$. The **input variables** are the flow rate $\phi_{V}$ and the inlet concentrations $c_{A,in}$ and $c_{X,in}$.

Of these inputs, $\phi_{V,in}$ is the **manipulated variable**: the problem statement tells us directly that it is set by a flow controller, i.e.\ it can be actively adjusted to influence the process. The inlet concentrations $c_{A,in}$ and $c_{X,in}$ are **disturbance variables**: they are set by an upstream process (e.g.\ the feed preparation) and are not adjustable by a controller acting on this reactor.
```

```{exercise}
:label: ExerciseLin3
Set up the species mass balances for A and X. Specify the initial conditions.
```
```{solution} ExerciseLin3
:label: SolutionLin3
:class: dropdown
Using $\phi_V$ for both the inlet and outlet flow rate:  
$
\frac{dc_A}{dt} = \frac{\phi_V}{V}\left(c_{A,in} - c_A\right) - k_r c_A c_X, \quad c_A(0) = c_{A,0}
$  
$
\frac{dc_X}{dt} = \frac{\phi_V}{V}\left(c_{X,in} - c_X\right) - k_r c_A c_X, \quad c_X(0) = 0
$
```

```{exercise}
:label: ExerciseLin4
Assume the process reaches a steady-state operating point $(\bar{c}_A, \bar{c}_X)$. Write down the algebraic equations that define these steady-state concentrations, and solve them to find explicit expressions for $\bar{c}_A$ and $\bar{c}_X$.
```
```{solution} ExerciseLin4
:label: SolutionLin4
:class: dropdown
At steady state, the derivatives are zero:  
$
0 = \frac{\phi_V}{V}\left(c_{A,in} - \bar{c}_A\right) - k_r \bar{c}_A \bar{c}_X \tag{1}
$  
$
0 = \frac{\phi_V}{V}\left(c_{X,in} - \bar{c}_X\right) - k_r \bar{c}_A \bar{c}_X \tag{2}
$

Subtracting (2) from (1) eliminates the reaction term:  
$
\frac{\phi_V}{V}\left(c_{A,in} - \bar{c}_A\right) - \frac{\phi_V}{V}\left(c_{X,in} - \bar{c}_X\right) = 0
\quad\Longrightarrow\quad
\bar{c}_A - \bar{c}_X = c_{A,in} - c_{X,in}.
$

For brevity, define $\Delta \equiv c_{A,in} - c_{X,in}$, so that $\bar{c}_A = \bar{c}_X + \Delta$. Substituting this into equation (2):  
$
0 = \frac{\phi_V}{V}\left(c_{X,in} - \bar{c}_X\right) - k_r\left(\bar{c}_X + \Delta\right)\bar{c}_X,
$  

which rearranges into a quadratic equation in $\bar{c}_X$:  
$
k_r \bar{c}_X^2 + \left(k_r \Delta + \frac{\phi_V}{V}\right)\bar{c}_X - \frac{\phi_V}{V}c_{X,in} = 0.
$

Solving with the quadratic formula and keeping the physically relevant (non-negative) root:  
$
\boxed{\bar{c}_X = \frac{-\left(k_r \Delta + \dfrac{\phi_V}{V}\right) + \sqrt{\left(k_r \Delta + \dfrac{\phi_V}{V}\right)^2 + 4k_r\dfrac{\phi_V}{V}c_{X,in}}}{2k_r}}
$  

and consequently  
$
\boxed{\bar{c}_A = \bar{c}_X + \Delta = \bar{c}_X + c_{A,in} - c_{X,in}.}
$
```

```{exercise}
:label: ExerciseLin5
The mass balance for X is nonlinear due to the reaction term. Linearize this equation using a multi-variable Taylor series expansion around the steady-state operating point $(\bar{c}_A, \bar{c}_X, \bar{\phi}_V, \bar{c}_{A,in}, \bar{c}_{X,in})$, and introduce deviation variables (e.g. $c_A'(t) = c_A(t) - \bar{c}_A$) to express the result.
```
```{solution} ExerciseLin5
:label: SolutionLin5
:class: dropdown
The mass balance for X derived in part {numref}`ExerciseLin3` can be written as a nonlinear function $f$ of the two state variables ($c_A, c_X$) and the three input variables ($\phi_V, c_{A,in}, c_{X,in}$):  
$
\frac{dc_X}{dt} = \frac{\phi_V}{V}\left(c_{X,in} - c_X\right) - k_r c_A c_X \equiv f(c_A, c_X, \phi_V, c_{A,in}, c_{X,in}).
$  

Note that $f$ does not actually depend on $c_{A,in}$ directly, since $c_{A,in}$ only enters the mass balance for A; we include it here as an argument of $f$ for generality, and its partial derivative will simply turn out to be zero.

A first-order Taylor series expansion of $f$ around the steady-state operating point \\$(\bar{c}_A, \bar{c}_X, \bar{\phi}_V, \bar{c}_{A,in}, \bar{c}_{X,in})$ approximates $f$ as its value at steady state, plus a correction term for each variable that is proportional to how far that variable has moved from its steady-state value:  
$
f \approx f_{ss} + \left(\frac{\partial f}{\partial c_A}\right)_{ss}\!\!(c_A - \bar{c}_A) + \left(\frac{\partial f}{\partial c_X}\right)_{ss}\!\!(c_X - \bar{c}_X) + \left(\frac{\partial f}{\partial \phi_V}\right)_{ss}\!\!(\phi_V - \bar{\phi}_V)$  
$+ \left(\frac{\partial f}{\partial c_{A,in}}\right)_{ss}\!\!(c_{A,in} - \bar{c}_{A,in}) + \left(\frac{\partial f}{\partial c_{X,in}}\right)_{ss}\!\!(c_{X,in} - \bar{c}_{X,in}).
$

By definition, at steady state the derivative is zero, so $f_{ss} = 0$. Introducing deviation variables (e.g.\ $c_A' = c_A - \bar{c}_A$), and using $\frac{dc_X}{dt} = \frac{dc_X'}{dt}$ since $\bar{c}_X$ is constant, the linearized equation becomes:  
$
\frac{dc_X'}{dt} = \left(\frac{\partial f}{\partial c_A}\right)_{ss}\!\!c_A' + \left(\frac{\partial f}{\partial c_X}\right)_{ss}\!\!c_X' + \left(\frac{\partial f}{\partial \phi_V}\right)_{ss}\!\!\phi_V' + \left(\frac{\partial f}{\partial c_{A,in}}\right)_{ss}\!\!c_{A,in}' + \left(\frac{\partial f}{\partial c_{X,in}}\right)_{ss}\!\!c_{X,in}'.
$  

Now we evaluate each partial derivative at the steady state.

**1. Derivative with respect to $c_A$:**  
$
\frac{\partial f}{\partial c_A} = -k_r c_X \quad\Longrightarrow\quad \left(\frac{\partial f}{\partial c_A}\right)_{ss} = -k_r \bar{c}_X.
$

**2. Derivative with respect to $c_X$:**  
$
\frac{\partial f}{\partial c_X} = -\frac{\phi_V}{V} - k_r c_A \quad\Longrightarrow\quad \left(\frac{\partial f}{\partial c_X}\right)_{ss} = -\frac{\bar{\phi}_V}{V} - k_r \bar{c}_A.
$

**3. Derivative with respect to $\phi_V$:**  
$
\frac{\partial f}{\partial \phi_V} = \frac{1}{V}\left(c_{X,in} - c_X\right) \quad\Longrightarrow\quad \left(\frac{\partial f}{\partial \phi_V}\right)_{ss} = \frac{1}{V}\left(\bar{c}_{X,in} - \bar{c}_X\right).
$

**4. Derivative with respect to $c_{A,in}$:**  
$
\left(\frac{\partial f}{\partial c_{A,in}}\right)_{ss} = 0,
$  
since $c_{A,in}$ does not appear in $f$ at all.

**5. Derivative with respect to $c_{X,in}$:**  
$
\frac{\partial f}{\partial c_{X,in}} = \frac{\phi_V}{V} \quad\Longrightarrow\quad \left(\frac{\partial f}{\partial c_{X,in}}\right)_{ss} = \frac{\bar{\phi}_V}{V}.
$

**Final linearized equation:**  
$
\boxed{\frac{dc_X'}{dt} = \left(-k_r \bar{c}_X\right)c_A' + \left(-\frac{\bar{\phi}_V}{V} - k_r \bar{c}_A\right)c_X' + \frac{1}{V}\left(\bar{c}_{X,in} - \bar{c}_X\right)\phi_V' + \frac{\bar{\phi}_V}{V}c_{X,in}'}
$
```

### Exercise B

Consider a continuously stirred tank reactor (CSTR) with a constant volume $V = 2000$ L, where a first-order, liquid-phase exothermic reaction ($A \rightarrow B$) takes place. The feed contains pure A and enters the reactor at a volumetric flow rate $q = 300$ L/min with an inlet concentration $C_{A0} = 4.0$ mol/L and a feed temperature $T_0 = 350$ K. The reactor is operated isothermally at a steady-state temperature of $\bar{T} = 350$ K, which is controlled by a cooler with an adjustable cooling duty $Q$. 

The following data are available:  
* Heat capacity of the mixture, $C_p = 3.50$ J/(g K)
* Density of the mixture, $\rho = 1.15$ g/cm$^3$
* Enthalpy of reaction, $\Delta H_R = -50.00$ kJ/mol
* Reaction rate constant, $k(T) = 2.4 \cdot 10^{15} e^{-12000/T}$ min$^{-1}$ (with $T$ in Kelvin)

```{exercise}
:label: ExerciseLinB1
Identify the state variables, the manipulated/controlled variables, and the disturbance variables of this process.
```
```{solution} ExerciseLinB1
:label: SolutionLinB1
:class: dropdown
**State variables:** Reactor concentration $C_A$, Reactor temperature $T$.  
**Manipulated variable:** Cooling duty $Q$.  
**Disturbance variables:** Feed temperature $T_0$, Feed concentration $C_{A0}$, Flow rate $q$.
```

```{exercise}
:label: ExerciseLinB2
The conversion of component A is 95.4\%. Determine the cooling duty $Q$ (in kJ/min) required to maintain the reactor isothermally at 350K. 
```
```{solution} ExerciseLinB2
:label: SolutionLinB2
:class: dropdown
Set up the steady-state energy balance to find $Q$. Let $Q$ be the heat added to the system (negative means cooling). Note that $\rho = 1.15 \text{ g/cm}^3 = 1150 \text{ g/L}$:  
$ 0 = q \rho C_p (T_0 - \bar{T}) + (-\Delta H_R) V k(\bar{T}) \bar{C}_A + Q $

Because the feed enters at 350K and the reactor operates at 350K, the sensible heat term ($T_0 - \bar{T}$) is zero. Additionally, the reactor concentration $\bar{C}_A=(1-0.954)\times C_{A0}=0.184$ mol/L:  
$ 0 = 0 + (50.00 \text{ kJ/mol})(2000 \text{ L})(3.09 \text{ min}^{-1})(0.184 \text{ mol/L}) + Q $  
$ 0 = 56,856 \text{ kJ/min} + Q \implies Q = -56,856 \text{ kJ/min} $

The reactor requires a **cooling duty** of **56,876 kJ/min**.
```

```{exercise}
:label: ExerciseLinB3
To design a temperature controller, we must understand how the temperature responds to changes. Set up the nonlinear differential equation for the reactor temperature $T$. Then, linearize this equation using a multi-variable Taylor series expansion around the steady-state operating point to show how the rate of change of the reactor temperature depends on small deviations in **all** state and input variables.
```
```{solution} ExerciseLinB3
:label: SolutionLinB3
:class: dropdown
The nonlinear differential equation for the reactor temperature is:  
$
 \frac{dT}{dt} = \frac{q}{V}(T_0 - T) + \frac{(-\Delta H_R)}{\rho C_p} k(T) C_A + \frac{Q}{\rho V C_p} 
$

Let's define the right-hand side as a nonlinear function $f$ of the state variables ($T, C_A$) and input variables ($q, T_0, Q$):  
$ \frac{dT}{dt} = f(T, C_A, q, T_0, Q) $

A first-order Taylor series expansion around the steady-state operating point $(\bar{T}, \bar{C}_A, \bar{q}, \bar{T}_0, \bar{Q})$ is:  
$ f \approx f_{ss} + \left( \frac{\partial f}{\partial T} \right)_{ss} (T - \bar{T}) + \left( \frac{\partial f}{\partial C_A} \right)_{ss} (C_A - \bar{C}_A) + \left( \frac{\partial f}{\partial q} \right)_{ss} (q - \bar{q}) + \left( \frac{\partial f}{\partial T_0} \right)_{ss} (T_0 - \bar{T}_0) + \left( \frac{\partial f}{\partial Q} \right)_{ss} (Q - \bar{Q}) $

By definition, at steady state, the derivative is zero, so $f_{ss} = 0$. Introducing deviation variables (e.g., $T' = T - \bar{T}$), the linearized equation becomes:  
$ \frac{dT'}{dt} = \left( \frac{\partial f}{\partial T} \right)_{ss} T' + \left( \frac{\partial f}{\partial C_A} \right)_{ss} C_A' + \left( \frac{\partial f}{\partial q} \right)_{ss} q' + \left( \frac{\partial f}{\partial T_0} \right)_{ss} T_0' + \left( \frac{\partial f}{\partial Q} \right)_{ss} Q' $

Note that $ \frac{dT}{dt}=\frac{dT'}{dt}$. Now, we evaluate each partial derivative at the steady state.

**1. Derivative with respect to Reactor Temperature ($T$):**  
$ \frac{\partial f}{\partial T} = -\frac{q}{V} + \frac{(-\Delta H_R)}{\rho C_p} C_A \frac{\partial k(T)}{\partial T} $

Using the chain rule on $k(T) = 2.4 \cdot 10^{15} e^{-12000/T}$:  
$ \frac{\partial k(T)}{\partial T} = k(T) \cdot \frac{12000}{T^2} $

Evaluating at steady state ($\bar{T} = 350$ K, $\bar{C}_A = 0.185$ mol/L, $\bar{q} = 300$ L/min, $k(\bar{T}) = 3.09$ min$^{-1}$):  
$ \left( \frac{\partial k}{\partial T} \right)_{ss} = 3.09 \cdot \frac{12000}{350^2} = 0.3027 \text{ min}^{-1} \text{K}^{-1} $  
$ \left( \frac{\partial f}{\partial T} \right)_{ss} = -\frac{300}{2000} + \frac{50.00}{(1.15)(3.50)} (0.185) (0.3027) = -0.15 + 0.695 = 0.545 \text{ min}^{-1} $

**2. Derivative with respect to Concentration ($C_A$):**  
$ \left( \frac{\partial f}{\partial C_A} \right)_{ss} = \frac{(-\Delta H_R)}{\rho C_p} k(\bar{T}) = \frac{50.00}{(1.15)(3.50)} (3.09) = 38.38 \text{ K L mol}^{-1} \text{min}^{-1} $

**3. Derivative with respect to Flow Rate ($q$):**   
$ \left( \frac{\partial f}{\partial q} \right)_{ss} = \frac{1}{V}(\bar{T}_0 - \bar{T}) = \frac{1}{2000}(350 - 350) = 0 $

**4. Derivative with respect to Feed Temperature ($T_0$):**  
$ \left( \frac{\partial f}{\partial T_0} \right)_{ss} = \frac{\bar{q}}{V} = \frac{300}{2000} = 0.15 \text{ min}^{-1} $

**5. Derivative with respect to Heat Duty ($Q$):**  
$ \left( \frac{\partial f}{\partial Q} \right)_{ss} = \frac{1}{\rho V C_p} = \frac{1}{(1150 \text{ g/L})(2000 \text{ L})(3.50 \text{ J/(g K)})} = 1.24 \cdot 10^{-7} \text{ K/J} $

**Final Linearized Equation:**  
$ \boxed{\frac{dT'}{dt} = 0.545 T' + 38.38 C_A' + 0.15 T_0' + (1.24 \cdot 10^{-4}) Q'_{kJ}} $
```
