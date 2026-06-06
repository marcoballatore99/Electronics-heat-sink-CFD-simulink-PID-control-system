# Electronics-heat-sink-CFD-simulink-PID-control-system
This project combines a CFD simulation of an air cooled heat sink for electronics cooling. The heat transfer coefficient correlation is extracted from a parametric analysis and imported in a simulink model for a PID controller to maintain the chip temperature below temperature limit threshold.

# CFD model setup
The CFD model is made in COMSOL 6.3. The geometry is created in COMSOL, and can be downloaded from the files. The overall simulation domain is composed by the heat sink, a chip (heat source), and the fluid domain. The fluid domain covers all of the heat sink, but not the chip. 

<img width="781" height="357" alt="COMSOL_domain_CFD" src="https://github.com/user-attachments/assets/1c59baad-a21c-4ab1-804b-0ca79c308385" />

<img width="781" height="357" alt="Mesh" src="https://github.com/user-attachments/assets/043f6d36-c33c-4300-8645-409283efdd07" />


The heat source of the chip is a constant heat rate of 10 W. The flow is modelled as laminar flow, and a parametric study is run for the velocity from 0.2 m/s to 2 m/s. The fluid considered is air, and the far field temperature is set to 20 degC. For each simulation and velocity, the maximum, minimum and average temperature of the heat sink is extracted. 

<img width="781" height="357" alt="Temperature" src="https://github.com/user-attachments/assets/55aa071b-c13c-4d07-8619-032b0808bd11" />

Through the parametric analysis, a heat transfer coefficient vs inlet air velocity correlation is extrapolated in excel. You can find the excel sheet in the files. 

# Simulink model
The correlation is then used in matlab simulink to develop a PID controller model. Firstly, the heat sink is modelled as two thermal masses (the chip and the heat sink), as well as a heat flux source (representing the heat source), a conductive heat transfer model (conduction through the chip), and a convective heat transfer model (conduction + convection through the heat sink). The CFD model, and the correspondent heat transfer convection correlation, allows to strongly simplify the heat sink in the simulink model, reducing it to a lumped mass and a global heat transfer coefficient between the air and the chip. 

<img width="965" height="657" alt="image" src="https://github.com/user-attachments/assets/f3dd178c-b9bd-4f85-9e00-080ef94c3fd1" />

In the simulink model, the heat transfer as a function of air velocity correlation is modelled through a matlab function f(u), which simply inputs the air velocity and returns the heat transfer coefficient h [W/m2K]. In the simulink model, to test the PID and a realistic situation, the chip heat source is modelled as variable in time through a matlab function.

# PID setup
A PID is a fundamental control system in engineering. It controls the output of a system (the power of the fan in this case) in order to mantain a certain variable (chip temperature) as close as possible to a predefined setpoint (operating temperature). A PID is controlled by three constants: Proportional, Integral, and Derivative (PID). The proportional gain Kp gives an output that is directly proportional to the error between the setpoint and the current variable. The integral gain Ki accumulates of the error over time. The derivative gain Kd anticipates the future trend based on the derivative of the error in time.

<img width="442" height="83" alt="image" src="https://github.com/user-attachments/assets/e074d943-d95d-4049-a203-30a6673a975a" />

<img width="2208" height="1124" alt="PID_temperature" src="https://github.com/user-attachments/assets/88a68461-f6f8-4553-9a5d-ca6b550f45aa" />




