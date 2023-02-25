# Lift-logical-networks-with-logisim
 Project realized for Tongji University.
 The project is realized by Lapo Carrieri and Filippo Castorri.
### Problem 
![image](https://user-images.githubusercontent.com/56505429/221370123-950b09c9-6df0-49d6-9d72-a9ba9dc797d8.png)

Design of a lift with logisim that is an educational tool for designing and simulating digital logic circuits.
### Resolution
The 2 outputs of the 2 Dflip-flops are the 2 that define the present floor; this 2 bits  are merged together  thanks to a splitter in only 1 signal of 2 bits that goes to different parts of the circuit. 
It goes to the HEX DIGITAL LED ![image](https://user-images.githubusercontent.com/56505429/221370005-d2264b06-3f90-48b9-a500-cb5d329cdfd6.png)
 that shows to the passengers the lift’s present floor(we use an adder to add  1 because it had to consider that the 00 is the first floor (i.e. 00+01=01 that is the present the first floor, the same thing is done with other floor); ![image](https://user-images.githubusercontent.com/56505429/221369993-c5fa97fa-880f-4c7e-a28c-46483f2a2c45.png)
 Also the “present state signal” goes to the input of the OPEN DOOR DECODER and of the CLOSE DOOR DECODER: it is necessary to ensure that the outputs of present floor FF activate only the right door in both situations (when doors have to be opened and when they have to be closed).
Then  there is the part of  the circuit that calculate how many floors the lift has to do. To calculate how many floor must be done two SUBTRACTOR and a MUX are necessary. The first SUB has the task to subtract present state minus next state, while the second one has to do next state minus present state. Then the MUX has the outputs of the two SUBTRACTOR as two inputs.![image](https://user-images.githubusercontent.com/56505429/221370042-3a6a1a0e-1f72-4ec4-b6b2-a6c285523a4c.png)
 The control input of this MUX is the verse of the lift, obviously if the lift has to go up the path that is chosen is the second one (next state minus present state) while if the lift has to go down the path chosen is the first one (present state minus next state), otherwise an error would occur in the calculation of floor to do.
![image](https://user-images.githubusercontent.com/56505429/221370036-bdc81cba-0ab6-43a1-9cd8-8e64e5b54a0f.png)

The signal of the number of the floors to do enter in the input of a DECODER that choose only one of the 3 outputs: if the lift must do 1 floor a COUNTER of 5 ticks starts, if the lift must do 2 floor a COUNTER of 10 ticks starts, if the lift must do 3 floor a COUNTER of 15 ticks starts. While one of the counters is active the motor is active. 
When one of the counter active finish, a signal clear the counter and a signal goes to an OR gate. Depending on which counter is enabled, when this counter finishes counting the lift has arrived to the right floor and so the OR gate is activate and this signal goes to different parts of the circuit. ![image](https://user-images.githubusercontent.com/56505429/221370065-bc905d85-00d6-4853-b49c-39cef21f097a.png)

It goes to the enable of the “open door decoder” to open the right door to the passengers; 
to slow down the arrival of the signal we use 2 D flip flops,In fact the signal of the end of the counter goes also in the clock of the 3 flip flops that describe the present floor and the direction of the lift; so the input of the open door decoder could be changed and  so there is a time problem and we solve this adding 2 flip flops. In this way the signal of 1 arrive 2 ticks later and so it can open the right door chosen by the input that is the present floor, so only the right door opens.
Also the output of the OR-gate goes to the clear of the “motor register” to turns off the motor, because the lift has arrived to the right floor and it is necessary to disable the motor.
Also the ouput of the OR-gate goes to the register on the right, it keeps the value (infact when the counter has finished its work the output change from 1 to 0 because the signal is instantaneous and this brings to a problem because we need a continuous signal). After this register there is a COUNTER that counts 10 ticks:the time to permitt the passegers to go out from the lift or to enter in the lift.
The outup of the counter:
 -Clear the previous register and the previous counter because we don’t need them anymore and therefore it becomes available for the next order to the lift.
-Goes at the same time to the enable and the input of the “motor register” to enable the motor( only if the floors to do is greater than 0).
-Goes to the enable of the “close door decoder” to close the right door that is described by the present floor of the lift.


-Goes  at the same time to the input and the clock of another register(at the bottom of the circuit). The signal at the Clear-port comes from the OR gate after the 3 COUNTER. This register is actually initiated to 1 and goes to an AND gate, and if it is 0 it disable the MUX. Another input of the AND-GATE whose output goes to the enable of the MUX, this input is the inverse of the output of an AND-GATE whose inputs are the inverse of the outputs of the second registers of each that record the call of each floor. Infact if all the output’s register are 0 nobody has called the register and so the MUX must be 0 because the lift must be stopped. We need this precaution because when we have achieved all the formulas for the calculation of the next state we realized that they were too complex to be realized in a circuit because there were too many possible combinations. So we have exploited the fact that if nobody called the lift this did not start to simplify the formulas.
For example if the lift is at the third floor and nobody calls from the forth floor we are not interested if the first two floors calls the lift or not; the next state direction will be always down;in fact if they don’t call it, the lift doesn’t start so there is no problem and the equations are simplify. The output of this register is 0 meanwhile the door is opening and the door is closing. It is fundamental to enable it because  otherwise the MUX’s output can change during this time and so it is possible that one of the three counter starts and the motor also starts while the door is open and while the passengers are going out.  
Under this register there is an OR-GATE whose inputs are: the 3 ouputs of the decoder before the counters and the ouput of the register before the forth counter. So the output of this OR-GATE is 1 when one of the counters is active. It goes to a NOT-GATE and after an AND-GATE whose output goes in the clock of the 4 registers( the second registers that shows if someone call the lift or not)
So if the OR-GATE’s output is 1 the clock remains 0 and so the 1 of the first register can’t go to the second register.
![image](https://user-images.githubusercontent.com/56505429/221370087-b60989c4-6217-4d38-a2ec-3f055472f479.png)

We need this gate because if the second register changes while the lift is active there are many problems, for example the next floor of the lift change. So we need that the request of the person out of the lift goes in the circuit only if the lift is stopped and all the doors are closed.

![image](https://user-images.githubusercontent.com/56505429/221370092-d1c32c25-c1d3-4040-92c5-339425382ef7.png)


 

