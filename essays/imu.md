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

## A novel implementation of ESKF for attitude estimation with a 6-DOF IMU


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
