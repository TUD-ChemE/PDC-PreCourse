# Input and State Variables

## Theory

Before setting up a model, it is useful to classify the variables in a process. A process is generally described by a set of **state variables**: quantities such as a concentration, temperature, or liquid level that describe the condition of the process at any given time. The state variables change as a result of **input variables**, quantities that act on the process from outside. Depending on whether or not an input can be adjusted by the control system, we distinguish between two types:
- **Manipulated variable:** an input variable that can be actively adjusted in order to influence the process. This could for example be the inlet flow rate, provided it can be dynamically adjusted (e.g., using a control valve).
- **Disturbance variable:** an input variable that affects the process but cannot be directly adjusted by the control system. Disturbances often originate outside the process and can be measured or unmeasured, and constant or time-varying. A common example is feed concentration, given that it is determined upstream of the process of interest.

Among the state variables, the one (or combination) whose value we want to keep at, or steer towards, a desired setpoint is called the **controlled variable**. This is typically a measured process output.

## Exercises

**Mass balance and species balance combined.**  
Consider a continuously stirred tank reactor (CSTR) with a volume $V_0$. The tank is filled up to a level that it contains a volume of $0.2V_0$ of water. At $t = 0$, a solution of salt in water with a concentration $c_0$ flows into the CSTR at a volumetric flow rate $Q$. At the same time, a valve is opened at the bottom of the tank through which fluid exits the CSTR at a rate of flow $Q/3$.

```{hint}
:class: dropdown
CSTR balance review is available through [Collegerama CRK Lecture 4](https://collegeramavideoportal.tudelft.nl/catalogue/4052chreky/presentation/fac410a62fad4f9fa89514e50c2aa00e1d?academicYear=2022-2023-4052chrek) at 1:24:00.
```

```{exercise}
:label: ExerciseISV1
Make a sketch of the problem, indicating all relevant variables.
```
`````{solution} ExerciseISV1
:label: SolutionISV1
:class: dropdown
```{figure} images/solutions/ex1sketch.png
Sketch of CSTR.
```
`````

```{exercise}
:label: ExerciseISV2
Identify the state variables and the disturbance variables of the process, given that both inlet and outlet flows cannot be controlled, and the concentration is set upstream of the process.
```
```{solution} ExerciseISV2
:label: SolutionISV2
:class: dropdown
The **state variables** are the volume of liquid in the tank, $V(t)$, and the salt concentration in the tank, $c(t)$: these together describe the condition of the process at any point in time. The **disturbance variables** are the inlet concentration $c_0$ and the inlet flow rate $Q$, since these are imposed by the upstream process and are not adjusted by anything within this system. The outlet flow rate, $Q/3$, is also a disturbance variable. Note that, as described, the process has no manipulated variable.
```

```{exercise}
:label: ExerciseISV3
Determine how the volume of liquid in the CSTR depends on time.
```
```{solution} ExerciseISV3
:label: SolutionISV3
:class: dropdown
Setting up the 'total mass' balance yields: $\frac{d}{dt} (\rho V)= \rho Q - \rho Q / 3$. Solving this differential equation with the initial condition $V(t=0) = 0.2 V_0$ gives $V(t) = 0.2 V_0 + \frac{2}{3} Q t$.
```

```{exercise}
:label: ExerciseISV4
Write down the differential equation from which the salt concentration at the exit of the CSTR can be computed.
```
```{solution} ExerciseISV4
:label: SolutionISV4
:class: dropdown
Setting up the 'component balance for species A' yields: $\frac{d}{dt}(Vc) = Q c_0 - Q c /3$.
```

```{exercise}
:label: ExerciseISV5
Use the differential equation in {numref}`ExerciseISV4` to determine how the salt concentration at the exit depends on time.
```
```{solution} ExerciseISV5
:label: SolutionISV5
:class: dropdown
Expand the left-hand side of the ODE from {numref}`ExerciseISV4` using the product rule:  
$
\frac{d}{dt}(Vc) = V\frac{dc}{dt} + c\frac{dV}{dt} = Qc_0 - \frac{Qc}{3}
$

From part {numref}`ExerciseISV3` we know $\dfrac{dV}{dt} = \dfrac{2}{3}Q$. Substituting this in:  
$
V\frac{dc}{dt} + \frac{2}{3}Qc = Qc_0 - \frac{Qc}{3}
$  

This gives a separable ODE:  

$$
\frac{dc}{dt} = \frac{Q(c_0-c)}{V(t)}, \qquad \text{with } V(t) = 0.2V_0 + \frac{2}{3}Qt.
$$

Separating variables:  
$
\frac{dc}{c_0-c} = \frac{Q\,dt}{0.2V_0 + \tfrac{2}{3}Qt}
$  

Integrate both sides. The right-hand side is of the form $\int \frac{Q}{a+bt}\,dt = \frac{Q}{b}\ln(a+bt)$, with $a = 0.2V_0$ and $b = \tfrac{2}{3}Q$, so $Q/b = 3/2$:  
$
-\ln(c_0 - c) = \frac{3}{2}\ln\!\left(0.2V_0 + \frac{2}{3}Qt\right) + C
$  

$$
c_0 - c = C_{\text{int}}\left(0.2V_0 + \frac{2}{3}Qt\right)^{-3/2}
$$  

Apply the initial condition $c(t=0) = 0$:  

$$
c_0 - 0 = C_{\text{int}}(0.2V_0)^{-3/2}
\quad\Rightarrow\quad
C_{\text{int}} = c_0(0.2V_0)^{3/2}
$$

Substitute back:  
$
c_0 - c = c_0\left(\frac{0.2V_0}{0.2V_0 + \tfrac{2}{3}Qt}\right)^{3/2}
= c_0\left(1 + \frac{2Qt}{3\cdot 0.2V_0}\right)^{-3/2}
= c_0\left(1 + \frac{10}{3}\frac{Q}{V_0}t\right)^{-3/2}
$

Dividing by $c_0$ and rearranging gives the final result:  
$
\boxed{\dfrac{c}{c_0} = 1 - \left(1 + \dfrac{10}{3}\dfrac{Q}{V_0}t\right)^{-3/2}}
$
```

```{exercise}
:label: ExerciseISV6
Suppose now that, instead of a constant outlet flow rate of $Q/3$, the outlet valve is such that the outlet flow rate depends on the liquid volume in the tank, according to  

$$
Q_{\text{out}}(t) = k V(t), \qquad \text{with } k = \frac{5}{3}\frac{Q}{V_0}
$$

where $k$ has been chosen such that $Q_{\text{out}}(0) = Q/3$, consistent with the original setup. Update the differential equation for the volume from part {numref}`ExerciseISV3` to account for this.
```
```{solution} ExerciseISV6
:label: SolutionISV6
:class: dropdown
With the outlet flow rate now given by $Q_{\text{out}}(t) = kV(t)$, the total mass balance from part {numref}`ExerciseISV3` is updated to:  

$$
  \frac{d}{dt}(\rho V) = \rho Q - \rho k V(t)
  \quad\Longrightarrow\quad
  \frac{dV}{dt} = Q - kV(t), \qquad \text{with } V(0) = 0.2V_0
$$
```

```{exercise}
:label: ExerciseISV7
Determine the steady state operating point $V_{ss}$: the volume at which the outlet flow rate equals the inlet flow rate $Q$. What does this operating point represent physically?
```
```{solution} ExerciseISV7
:label: SolutionISV7
:class: dropdown
“In
this case the operating point is at a steady state of the system, so the point where $\dfrac{dV}{dt} = 0$:  
$
  0 = Q - kV_{ss} \quad\Longrightarrow\quad V_{ss} = \frac{Q}{k} = \frac{Q}{\tfrac{5}{3}\tfrac{Q}{V_0}} = \frac{3}{5}V_0 = 0.6V_0
$
  
Physically, $V_{ss} = 0.6V_0$ is the liquid volume at which the outlet flow rate exactly balances the inlet flow rate, so that the volume no longer changes with time. Starting from $V(0) = 0.2V_0 < V_{ss}$, the volume will rise and approach $V_{ss}$ as $t \to \infty$, since the outlet flow $Q_{\text{out}} = kV$ grows with volume until it matches the constant inflow $Q$. This equilibrium is called the **operating point** of the process: a steady-state condition around which the system naturally settles.
```

```{exercise}
:label: ExerciseISV8
Suppose the tank has reached the operating point found in part {numref}`ExerciseISV7`, when a blockage forms in the outlet pipe. As a result, the flow coefficient drops to  

$
k' = \frac{4}{3}\frac{Q}{V_0}.
$

Determine the new steady state operating point $\bar{V_{ss}}$. Then determine how long it takes, from the moment the blockage occurs, for the volume to settle to within 5\% of this new operating point.
```
```{solution} ExerciseISV8
:label: SolutionISV8
:class: dropdown
Substituting $k' = \dfrac{4}{3}\dfrac{Q}{V_0}$ into the differential equation from part {numref}`ExerciseISV6` gives:  

$$
  \frac{dV}{dt} = Q - \frac{4}{3}\frac{Q}{V_0}V(t), \qquad \text{with } V(0) = 0.6V_0,
$$

where $V(0) = 0.6V_0$ is the volume at the moment the blockage occurs, i.e. the operating point found in part {numref}`ExerciseISV7`.

**New operating point:** Setting $\dfrac{dV}{dt} = 0$:  
$
  0 = Q - \frac{4}{3}\frac{Q}{V_0}V_{ss}' \quad\Longrightarrow\quad V_{ss}' = \frac{Q}{\tfrac{4}{3}\tfrac{Q}{V_0}} = \frac{3}{4}V_0 = 0.75V_0
$

**Solving for time:** The equation is separable:  
$
  \frac{dV}{Q - \tfrac{4}{3}\tfrac{Q}{V_0}V} = dt
$

Integrating both sides:  
$
  -\frac{3}{4}\frac{V_0}{Q}\ln\!\left(Q - \frac{4}{3}\frac{Q}{V_0}V\right) = t + C
$  

Apply the initial condition $V(0) = 0.6V_0$ at  $t=0$ to solve for $C$:  
$
  -\frac{3}{4}\frac{V_0}{Q}\ln\!\left(Q - \frac{4}{3}\frac{Q}{V_0}(0.6V_0)\right) = 0 + C
$

$
  -\frac{3}{4}\frac{V_0}{Q}\ln\!\left(Q - 0.8Q\right) = C
  \quad\Longrightarrow\quad
  -\frac{3}{4}\frac{V_0}{Q}\ln(0.2Q) = C
$  

Substituting $C$ back in and rearranging:
$
  \ln\!\left(\frac{0.2Q}{Q - \tfrac{4}{3}\tfrac{Q}{V_0}V}\right) = \frac{4}{3}\frac{Q}{V_0}t
  \quad\Longrightarrow\quad
  Q - \frac{4}{3}\frac{Q}{V_0}V = 0.2Q\,e^{-\tfrac{4}{3}\tfrac{Q}{V_0}t}
$  

Solving for $V(t)$:
$
  V(t) = \frac{3}{4}\frac{V_0}{Q}\left(Q - 0.2Q\,e^{-\tfrac{4}{3}\tfrac{Q}{V_0}t}\right)
  = \frac{3}{4}V_0\left(1 - 0.2\,e^{-\tfrac{4}{3}\tfrac{Q}{V_0}t}\right)
$  
$
  \boxed{V(t) = 0.75V_0 - 0.15V_0\,e^{-\tfrac{4}{3}\tfrac{Q}{V_0}t}}
$


The volume is within 5\% of its new operating point when the remaining gap $|V(t) - V_{ss}'|$ has shrunk to $5\%$ of $V_{ss}'$ itself:  
$
  0.15V_0\,e^{-\tfrac{4}{3}\tfrac{Q}{V_0}t_{5\%}} = 0.05(0.75V_0) 
$  
$
  0.15V_0\,e^{-\tfrac{4}{3}\tfrac{Q}{V_0}t_{5\%}} =  0.0375V_0
$  

Dividing both sides by $0.15V_0$:  
$
  e^{-\tfrac{4}{3}\tfrac{Q}{V_0}t_{5\%}} = 0.25
$  

Solving for $t_{5\%}$:  
$
 \boxed{t_{5\%} = \frac{\ln(4)}{\tfrac{4}{3}\tfrac{Q}{V_0}} = \frac{3}{4}\ln(4)\,\frac{V_0}{Q}}
$
```
