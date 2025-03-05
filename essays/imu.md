---
layout: essay
type: essay
title: "Attitude estimation with a 6-DOF IMU"
# All dates must be YYYY-MM-DD format!
date: 2025-02-17
published: true
labels:
  - State Estimation
  - Inertial Measurement Unit(IMU)
  - Error State Kalman Filter(ESKF)
---

## 


$P_{n}$ can be decomposed into such form  

$$P_{n}=U_{n}\Lambda_{n}U_{n}^T,$$  

where $\Lambda_{n}$ is a diagonal matrix $U_{n}$ is an orthonormal matrix which satsifies

$$U_{n} = \begin{bmatrix} g^{'}, & g^{''}, & g \end{bmatrix}, g^{T}g^{'}=0, g^{T}g^{''}=0, g^{T}g=1$$ 

Using Woodbury Matrix Identity, the correction procedure of Kalman filter can be written as  

$$P_{n+1}=(P_{n+1|n}^{-1}+H_{n}^{T}V_{n}^{-1}H_{n})^{-1}$$


$$\begin{align*}
P_{n+1} &= (P_{n+1|n}^{-1}+H_{n}^{T}V_{n}^{-1}H_{n})^{-1} \\
        &= (P_{n+1|n}^{-1}+(R_{n}^{T}g^{\wedge})^{T}V_{n}^{-1}(R_{n}^{T}g^{\wedge}))^{-1} \\
        &= (P_{n+1|n}^{-1}+\delta_{H}^{-2}(g^{\wedge})^{T}g^{\wedge})^{-1} \\
        &= (P_{n+1|n}^{-1}+\delta_{H}^{-2}diag\{1,1,0\})^{-1} \\
        &= (U_{n}\Lambda^{-1}U_{n}^{T}+\delta_{H}^{-2}U_{n}diag\{1,1,0\}U_{n}^{T})^{-1} ??sth\space wrong\\
        &= (U_{n}(\Lambda^{-1}+\delta_{H}^{-2}diag\{1,1,0\})U_{n}^{T})^{-1} \\ 
        &= U_{n}(\Lambda^{-1}+\delta_{H}^{-2}diag\{1,1,0\})^{-1}U_{n}^{T} \\
        &= U_{n}\begin{pmatrix} \\
        \frac{1}{\frac{1}{\lambda_{1}}+\frac{1}{\delta_{H}^{2}}} & 0 & 0\\ 
        0 &\frac{1}{\frac{1}{\lambda_{2}}+\frac{1}{\delta_{H}^{2}}} & 0 \\
        0 & 0 & \lambda_{3}\\
        \end{pmatrix}U_{n}^{T} \\
\end{align*}$$


## Parametrization of Error on S2 manifold

Given $u,v$ on $S2$ Manifold. We can parametrize the distance between $u,v$ as  

$$Ru=v,$$

where $R$ has a closed form as  

$$R=I+[u\times v]^{\wedge}+\frac{1-u\cdot v}{\left\| u \times v \right\|^2}([u\times v]^{\wedge})^2$$ 


$trace(R)=1+2u\cdot v$  
$\theta=\frac{1}{2}(trace(R)-1)=u\cdot v$  
$w=\frac{\theta}{2sin\theta}[R-R^T]^\vee=\frac{u\cdot v}{sin(u\cdot v)}(u\times v)$


This is a direct observation to the Lie algebra of the rotation in the state.  
Thus Hessain Matrix $H=I$  
The covariance matrix is acting on the space of Lie algebra of rotation.


## Attitude Estimation

### State Rrepresentation
The true state is  

$$ X= [R, b^g] $$  

$X\in{G}$ is a group with the operation  $\circ$

$$X_1\circ X_2=[R_1R_2, b^g_{1}+b^g_{2}]$$  

The Identity is $X_I=[I, \bold{0}]$.  

The nominal state which represents the mean of the true state is

$$ \overline{X}=[\overline{R}, \overline{b}^g] $$  

The error state which lies on a Euclidean Space is  

$$\delta{X}=[\delta{\theta}, \delta{b}^g]$$  

Now we define a lifting Map $\Lambda : L \rightarrow G$, where $L\subseteq{\mathbb{R}^n}$  

$$\Lambda(\delta{X}) = [exp(\delta{\theta}^\wedge), \delta{b}^g]$$

The diffential of $\Lambda$ is  

$$D|_{\delta{X}}\Lambda = [exp(\delta{\theta}^\wedge)\dot\delta{\theta}^\wedge, \dot\delta{b^g}]$$  

And the reverse lifting map is  

$$\Lambda^{-1}(X)=[log(R)^\vee, b^g]$$

Hence, the error state can be further defined as   

$$X=\Lambda(\delta{X}) \circ \overline{X}$$  

### Error State Dynamics 
Attitude dynamics can be written as  

$$\dot{R}=R(\omega-b^g-n^g)^\wedge$$

$$\dot{b^g}=n^{b^g}$$

where $\omega$ is the angular velocity, $b_g$ is the bias of the gyroscope and $n^g,n^{b^g}$ are the noise of the gyroscope and the noise of the bias of the gyroscope respectively.  
The observation model uses accelerometer as the system output.  

$$z_a=R^Tg$$

where $g$ is the unit gravity vector.  
The true state dynamics is  

$$\dot{X}=[R(\omega-b^g-n^g)^\wedge,n^{b^g}]$$  

The nominal state dynamics is  

$$\dot{\overline{X}}=[\overline{R}(\omega-\overline{b}^g)^\wedge,\bold{0}]$$  

The error state dynamics is  

$$\dot{\delta{X}}= D\Lambda^{-1}(X\circ \overline{X}^{-1})$$  











