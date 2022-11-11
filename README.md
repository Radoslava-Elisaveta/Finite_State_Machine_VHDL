# Finite_State_Machine_VHDL
Implementong finite state machine using VHDL
This finite state machine was integrated with Elbert V2 – Spartan 3A FPGA and implemented combinations of output signals according to the table

![image](https://user-images.githubusercontent.com/76002002/201373650-98f542bd-8f9f-4bee-a5ca-5fcf2ea40f46.png)

The device must use the clock signal from the microcontroller and reduce the frequency using an internal divider. The microcontroller is part of the stand. The clock signal is connected to the input LOC = P129 of the FPGA

The device interface must have a synchronous reset input

The device interface must have a mode control input

If MODE = 0, the state of the device is incremented by the rising edge of the state memory clock signal

If MODE = 1, the state of the device is decremented on the rising edge of the state memory clock signal

The device interface must have a single-bit speed control input SPEED
If SPEED = 0, the machine works at the default speed

If SPEED = 1, the machine works at a speed TWICE HIGHER than in the mode

For a more optimal description of the logic interface, a table (table 1) of all input values and possible changes of the MODE control signal was created
M denotes the MODE signal, Q0-Q2 – initial states, Qt1-Qt2 – subsequent states, we will describe these functions

tabel 1

M	  Q2	Q1	Q0	Qt2	Qt1	Qt0

0 	0	  0	  0	  0	   0	1

0	  0	  0	  1	  0	   1	0

0	  0	  1	  0	  0	   1	1

0	  0	  1	  1	  1	   0	0

0	  1	  0	  0	  1	   0	1

0	  1	  0	  1	  1	   1	0

0	  1	  1	  0	  1	   1	1

0	  1	  1 	1	  0	   0	0

1	  0	  0	  0	  1	   1	1

1	  1	  1 	1	  1	   1	0

1	  1	  1	  0	  1  	 0	1

1	  1	  0	  1	  1	   0	0

1	  1	  0	  0	  0	   1	1

1	  0	  1	  1	  0	   1	0

1	  0	  1	  0	  0	   0	1

1	  0	  0	  1	  0	   0	0

Function minimization using Karnaugh maps
![image](https://user-images.githubusercontent.com/76002002/201374142-a0594b16-ddf6-4276-9f4b-69618bfb89d2.png)


   
Qt2 = /M*Q2*/Q1+M*/Q2*/Q1*/Q0+M*Q2*Q0+/M*/Q2*Q1*Q0+Q2*Q1*/Q0
Qt1 = M*/Q1*/Q0+/M*/Q1*Q0+M*Q1*Q0+/M*Q1*/Q0
Qt0 = /Q0*/Q1 + Q1*/Q0

To create the main frequency divider, two 16-bit counters with asynchronous reset were used. Also, a 2-digit counter was additionally used to create a signal controlling the speed of the machine (SPEED).
When SPEED = 0, the signal "1" is applied to the control input of the 2-bit counter through the logic gate AND, the counter starts its work, the TC transfer output is a logical unit, and the frequency division continues through the OR logic element.
When SPEED = 1, the signal "0" is applied to the control input of the 2-bit counter through the logic gate AND, the 2-bit counter does not work, the synchro pulse signal from the CLOCK input is applied to the sync pulse input C of the 16-bit counter through the logic gate AND. Thus, the dividing factor of the input frequency is reduced by 2, and the speed of the automaton is doubled.

Complete finite stame machine

![image](https://user-images.githubusercontent.com/76002002/201375719-6505e38d-60c5-4d57-b7ba-1d931acfa8af.png)

