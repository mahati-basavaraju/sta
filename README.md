# Static Timing Analysis - I

## Static Timing Analysis

###  Section 1 - Introduction and Agenda

### Checks

It is very important to identify a valid timing path.

A timing path should have a start point and an end point.
Start point is either Flop clk pin or input ports
End point is Flop d pin/ output ports

Refer to below image to get an idea of various timing paths in a circuit.

<p align='center'>
<img src='https://user-images.githubusercontent.com/110677094/190868925-005d6e45-11a2-4312-b4e0-a9da9f58262f.png'/>
</p>

The abovee timing diagram shows 4 different timing paths


Arrival Time

Time required for a signal to reach at the end point from the start point.

<p align='center'>
<img src='https://user-images.githubusercontent.com/110677094/190869215-f4e9848f-7957-4422-b2cd-ca50209666f1.png' />
</p>
When there are more than one possible start points for a particular end point, calculating the arrival point becomes tricky.

Required Time

Required time defines the needs of the system or the specifications of the system.
Required time is the expected time for the signal to arrive.

For example,

<p  align='center'>
<img src='https://user-images.githubusercontent.com/110677094/190869331-7b0bfbab-54c6-4bf6-9e9c-ee9e5cc31346.png'/>
</p>

These values are specified by the constraints and are the specifications to be met.

Slack

At a broad level, slack is the difference between the arrival time and the expected time.

<p align='center'>
<img src='https://user-images.githubusercontent.com/110677094/190869410-4cd8be29-c27a-4983-9ac7-9ba751befb5f.png'/>
</p>

In the above example, if we consider that the Signal should arrive after 0.5 ns and before 3ns, it implies that the arrival time should lie between 0.5 and 3 ns. Hence 0.5 ns can be considered as the minimum expected time and 3 ns can be considered as the maximum expected time. Also,let us suppose that the arrival time is 3.5ns.
In this case, it satisfies one of the conditions and does not satisfy the other.

It meets the min constraint and does not meet the max constraint.

Min difference or min slack = Arrival time - Required time = 3.5 - 0.5 ns = 3ns

Refer below image
![image](https://user-images.githubusercontent.com/110677094/190869634-f4a8305f-7e96-469e-b3a5-c780616b8d0a.png)

Max difference or max slack = Required time - arrival time = 3 - 3.5 = -0.5 ns  --> This means that the signal is delayed by 0.5 ns and it needs to be taken care of.

Refer below image
![image](https://user-images.githubusercontent.com/110677094/190869648-ad01de85-9ba8-4978-873e-7dd149bc61a4.png)

Considering another example where min difference or minimum slack is not met. Refer below images.

![image](https://user-images.githubusercontent.com/110677094/190869700-0c68cbf2-d5d1-4079-8502-25a23d0b79ef.png)

![image](https://user-images.githubusercontent.com/110677094/190869704-cfc13f54-189f-4f75-8082-1e10a303652a.png)


Max slack is also referred to as setup slack or setup timing or setup analysis
Min slack is referred to as hold slack or hold timing or hold analysis

So finally if the signal does not arrive within the given specifications, then it means there are some violations.

### Types of Setup/ Hold analysis

1. reg2reg

Any path crossing from a register to register. In the below example, the path is from a launch flop to a capture flop

![image](https://user-images.githubusercontent.com/110677094/190869893-314ac95c-ec3c-4bfd-a161-bd2ae1b257d4.png)


2. in2reg

Timing path between input port (not the clock ports) to D pin of the launch flop

![image](https://user-images.githubusercontent.com/110677094/190869943-da7b6cf8-3c40-4d88-b193-b3dec52a2031.png)

3. reg2out

Timing path from capture flop output to output port

![image](https://user-images.githubusercontent.com/110677094/190869981-2fcf3bd4-2352-434b-93b3-f1218afb00e0.png)


4. in2out

Timing path from input port to output port

![image](https://user-images.githubusercontent.com/110677094/190869995-cd04e5dc-c8a7-4386-bbe3-6d86a866d6c5.png)

There are many more categories for analysis

To understand the other categories, we finetune the circuit to include clock gating. the capture flop is not given the clock directly but through another circuit. The clock gating technique is widely used to to reduce the amount of chip power.

5. clock gating 

![image](https://user-images.githubusercontent.com/110677094/190870288-3c8a511b-7d35-4168-b2fb-1a72a32619b9.png)

As there is a new clock, there is a new timing path

Whenever the gating_signal is high, clock is given to the capture flop and when the gating_signal is 0, clock is not supplied to the capture flop.

This now involves a new category of analysis, known as clock gating analysis for the path between the new clock and the AND gate output

6. recovery/ removal

Considering asynchronous pins of the flop, for example, reset.

Recovery/ removal timing analysis involves the timing path is between the new clock and the reset pin. The reset signal should arrive after a particular time after the clock signal.

![image](https://user-images.githubusercontent.com/110677094/190870428-50578310-7d5f-4d4b-945f-ca2e7c604a49.png)

7. data-to-data

To save some amount of power in the reset path, the circuit is modified as below. A control signal is introduced to control the gating signal. Based on the A and control signal, the capture flop might be set or reset and hence some leakage power can be saved accordingly.

The specification is that the A and control signal should be in sync. The  A and the control pin should be treated as end points. Then we get different timing paths corresponding to that, as shown below. These timing paths need a data check analysis. A timing path between 2 data signals is considered as data-to-data analysis.

![image](https://user-images.githubusercontent.com/110677094/190870619-074bb265-b316-4fed-92db-6bcfd82ed261.png)

The requirement could be that the A signal should arrive later than control signal and this can be ensured with this kind of timing analysis. Few constraints could be applied to meet this requirement.

8. latch (time borrow/ time given)

Launch flop becomes transparent at the edge of the clock signal.
Latch becomes transparent at the level of the clock pulse.

![image](https://user-images.githubusercontent.com/110677094/190871000-bc4fd1e2-b97d-4bd4-a9c4-5a6ce8e25539.png)

If the timing path constraint from the flop to latch is not met, the flop can borrow some time from the area of level of the clock signal.

![image](https://user-images.githubusercontent.com/110677094/190871012-9b920580-cc03-4ae1-b37a-1550c33673c4.png)


If the timing path constraint from the latch to the flop is not being met, some time can be given from the level of clock signal area to the start point i.e. the flop. 

![image](https://user-images.githubusercontent.com/110677094/190871025-c7ef2fa4-d4e6-4d63-ac65-be01de74444a.png)

We also have the slew/ transition analysis, which have to met i.e., should be in between a particular min or max value.

Slew/ Transiiton Analysis

They are of 2 types:
- Data (max/ min)
- Clock

It is because the data doesn't change as frequently as the clock does. The clock has to change at equal intervals. Hence the clock transition requirements are stringent as compared to that of the data.

Data slew is calculated as slew along different points on data paths and are checked across min/max values.

![image](https://user-images.githubusercontent.com/110677094/190871424-b7e8446d-9f86-47eb-9b1c-e56c54f5208e.png)

The clock signal sent through the clock port, traverses through the entrire clock network. Hence it is bound to degrade. Therefore, the clock transition requirements are stringent.
Slew is calculated across some points on the clock network and it values are verified across min/max values.

![image](https://user-images.githubusercontent.com/110677094/190871413-e108291e-33d7-45f6-b106-13683202d412.png)


Load Analysis

It specifies what is the load at each and every node and also if the node is able to charge or dischrge that amount of load.

Load analysis is of 2 types
- Fanout (max/min)
    If the fanout is 3, the amount of load on the node will increase if it is able to carry that amount of load. There is max and min value for every node which depends on the capacitance it can drive.
    ![image](https://user-images.githubusercontent.com/110677094/190871607-59c25d20-37fe-4ce9-b187-341ec570d92a.png)

- Capcitance (max/min)

    On the same end points, we can check the capcitances (if thery are within the required max and min values)
    
    ![image](https://user-images.githubusercontent.com/110677094/190871636-522440d8-3a71-4514-bdf2-6cd820b9c18b.png)

Clock analysis

It is broadly classified into

- Skew
Difference between different clock networks is the skew. Refer the below diagram
![image](https://user-images.githubusercontent.com/110677094/190871709-41807d7c-c262-4e8f-bd20-b385c4b52202.png)

- Pulse width

The clock pulse given at the clock port reaches the flops but with degradation due to parasitic capacitances. Pulse width analysis is required to understand how much can the clock pulse degarde for the circuit to still be working as exepcted. (i.e., if the clock pulse width reduces too much, then it might be not be even enough to load the data)

![image](https://user-images.githubusercontent.com/110677094/190871760-ad59ed25-2534-46ea-a01a-c87edda19747.png)


###  Section 2 - Introduction to timing graph

Types of setup/ hold analysis

1. reg2reg

Consider specifications as 

Clock frequency (F) = 1GHz
Clock Period (T) = 1/F = 1ns

and also consider below circuit for further analysis

![image](https://user-images.githubusercontent.com/110677094/191672073-3680ff02-e7ea-4cfd-b016-897870d67cfa.png)



Timing Analysis (with real clocks)

Setup Analysis - Single clock

Let's begin with the combinational logic

![image](https://user-images.githubusercontent.com/110677094/191672272-243cca0e-9e72-48f9-92fb-bdd6b7bcee1e.png)


Converting the combinational circuit into a Directed Acyclic graph (DAG). This is known as timing graph in STA.

Timing graph is a representation of the complete circuit in nodes. using timing grpaphs, delays and arrival times can be calculated easily.

![image](https://user-images.githubusercontent.com/110677094/191673218-c1cbce37-f264-4985-aa33-888317b07fff.png)


Note that the cell delay, wire delay and signal arrival time values are in units. (In real scenario, units are in pico seconds)

![image](https://user-images.githubusercontent.com/110677094/191674527-d3064813-1171-4e46-9eaa-d479cad24d19.png)

####Actual Arrival Time (AAT)

Calculating arrival times for all nodes

Few cell have more than one node as input, the arrival time for them is calculated as below

![image](https://user-images.githubusercontent.com/110677094/191675046-31c9ee53-d8f3-42a1-abb5-442e6bfdc350.png)

![image](https://user-images.githubusercontent.com/110677094/191675088-1eaa8fd2-59d7-46d9-ba3c-f6c6b5f4badc.png)

The selection of A1 or A2 depends on the type of analysis one is trying to do

Here since we are considering a reg2reg path, and the condition that arrival time should be less than max delay , we need to consider all worst case delays. Hence selecting A2 for the above example

![image](https://user-images.githubusercontent.com/110677094/191675509-6f5ce0a6-398b-4978-bfdf-26301f0ed4cd.png)

After calculating all the arrival times in a similar fashion, below is the circuit digram.

![image](https://user-images.githubusercontent.com/110677094/191675657-620fef90-94b6-4606-8acf-a72cc76796b2.png)

Slack is computed is based on above timing graph.

####Required Arrival Time (RAT)

![image](https://user-images.githubusercontent.com/110677094/191675958-ff7c62da-c6c1-4944-b5e6-6514d864ccc4.png)

The term 'expect at latest transition' implies that they are the constraints or the specifications.

Difference between AAT and RAT

![image](https://user-images.githubusercontent.com/110677094/191676220-67bf9f07-5e56-4cae-9c40-2a69af89a017.png)

![image](https://user-images.githubusercontent.com/110677094/191676283-337d4322-2186-4e7c-8424-ac4ef5e58dcd.png)

Where can this be helpful? To identify which node is giving the most negative slack if we see a negative slack at the output.

Let's assume or the system specification requires that the RAT isi 7.55 ns.
Calculating the RATs for all the all nodes as below.

![image](https://user-images.githubusercontent.com/110677094/191676736-ad3928ad-d314-4d0d-af0f-031f64cfde17.png)

![image](https://user-images.githubusercontent.com/110677094/191676799-4111db8b-f68a-405f-a712-df17bcf7825e.png)

Which RAT to consider? Take the best case.
Reason: If we consider the worst case to compute the slack i.e, required arrival time - actual arrival time , then there is a loss or optimism of few units.
(Will be explained in detail in next sections)

![image](https://user-images.githubusercontent.com/110677094/191677335-6d2ee47f-4cd1-49be-a4b8-fab2996ab2a3.png)

The slack is defined as 

![image](https://user-images.githubusercontent.com/110677094/191677494-3e5b74c2-6dad-42d0-b367-d0146a568684.png)

Negative slack is shown as timing constraints violated

One way to remove negative slack to use a cell with same functionality with a different cell delay so as to accomodate the timing constraints. This is what is done in ECO (Engineering Change order)

![image](https://user-images.githubusercontent.com/110677094/191679556-ec8b8e67-b590-4b81-8e46-c4874d3cfc17.png)

The nodes i2(0), a(2), c(3) and d(2) can be toned to get a positive slack.

Consider d node for further analysis

GBA - worst case analysis
PBA - considers actual paths that will be traced on silicon

In GBA, Slack is -0.35

![image](https://user-images.githubusercontent.com/110677094/191680402-60f86557-e283-46cc-8a9f-af86d478cb31.png)

In PBA, slack is +0.55

![image](https://user-images.githubusercontent.com/110677094/191680438-9c7fa71e-fc7a-4c24-b98b-f10b64fd1905.png)

But this comes with a certain cost, ie.e., time to do the calculations. There is a run time impact (Will be explained in later sections)

For accurate and detailed timing analysis, DAG should be with pin node conventions. (i.e, each pin , inputs and outputs will have its own AAT, RAT and Slack)

For pin node conventions, all pin information needs to be extracted.

![image](https://user-images.githubusercontent.com/110677094/191681410-add31771-8dac-406b-b9a0-9f5089e46bbc.png)

![image](https://user-images.githubusercontent.com/110677094/191681662-8595d8c5-3829-40fa-aaf0-0b9c0f282ac6.png)

###  Section 3 - Clk-to-q delay, library setup, hold time

Check the transistor level circuitry for launch flop and capture flop.

But why?



 
