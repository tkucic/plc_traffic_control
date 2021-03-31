# Traffic control simulator | [MAIN] | [NAMESPACES] | [METRICS] | [BACK]  

## Documentation for Program main  

```pascal
INTERFACE
    VAR
        TactTimer : TON; (*Timer to provide tact for all lights in a synchronized system*)
        vPeriodTime : TIME := TIME#1m2s0ms; (*Period after the sequence repeats*)
        vElapsedTime : TIME; (*Elapsed time of the sequence*)
        vTimeToSequenceChange : UDINT; (*Time in seconds to next sequence change*)
    END_VAR
END_INTERFACE
PROGRAM main:
    (*Functional description:
This is a program that needs to control two crossroads with cars and pedestrians. The crossroads are one way streets
so cars and pedestrians move in only one direction. It needs to control the traffic lights synchronously
so green wave is kept on the vertical and horizontal lines.
Pedestrian lights work opposite of the car lights so if the vertical line for the cars is green,
the horizontal pedestrians are not allowed to cross and vice versa.
For the pedestrians a time needs to be calculated to show when the green/red light is going to change.

This program implements the sequence diagram given below.
H R   0-30
H Y   28-30 && 60-62
H G   30-60
V R   28-60
V Y   26-28 && 58-60
V G   0-26  && 60-62
P H R 28-60
P H G 0-28
P V R 0-28
P V G 28-60

Simulator decription:
The simulator moves the objects on the road/side walk until they reach a decision point. Decision points
are located next to the related traffic lights. The objects will move only if the traffic light is green
for their direction.
In the simulator the fault monitoring is also active which will stop the objects moving and freeze the lights
so the operator can check where it went wrong. It will also display a message on the screen with the location
and fault reason. During collission or dangerous lights combination all red lights will blink.
To get through the fault situation the operator must enable police control which puts all lights in blinking yellow mode.*)

(*Start the timer and reset it after it expired. Basically count to 85s and then start over*)
TactTimer(IN:=TRUE, PT:=vPeriodTime, ET => vElapsedTime);
IF TactTimer.Q THEN
	TactTimer(IN:=FALSE);
END_IF
(*Calculate next sequence change*)
IF vElapsedTime <= T#28S THEN
	vTimeToSequenceChange := TIME_TO_UDINT(T#28S - vElapsedTime)/1000;
ELSIF vElapsedTime <= T#62S THEN
	vTimeToSequenceChange := TIME_TO_UDINT(T#62S - vElapsedTime)/1000;
END_IF
(* Direction Up, Down traffic light *)
gIO.Cross1CarU.RedLamp    := vElapsedTime >= T#0S  AND vElapsedTime <= T#30S;
gIO.Cross1CarU.YellowLamp := vElapsedTime >= T#28S AND vElapsedTime <= T#30S OR vElapsedTime >= T#60S AND vElapsedTime <= T#62S;
gIO.Cross1CarU.GreenLamp  := vElapsedTime >= T#30S AND vElapsedTime <= T#60S;

(* Mirror the other side of the Up, Down direction *)
gIO.Cross1CarD := gIO.Cross1CarU;
gIO.Cross2CarD := gIO.Cross1CarU;
gIO.Cross2CarU := gIO.Cross1CarU;

(* Direction Left, Right traffic light *)
gIO.Cross1CarL.RedLamp    := vElapsedTime >= T#28S AND vElapsedTime <= T#60S;
gIO.Cross1CarL.YellowLamp := vElapsedTime >= T#26S AND vElapsedTime <= T#28S OR vElapsedTime >= T#58S AND vElapsedTime <= T#60S;
gIO.Cross1CarL.GreenLamp  := vElapsedTime >= T#0S  AND vElapsedTime <= T#26S OR vElapsedTime >= T#60S AND vElapsedTime <= T#62S;

(* Mirror the other side of the Left, Right direction *)
gIO.Cross1CarR := gIO.Cross1CarL;
gIO.Cross2CarR := gIO.Cross1CarL;
gIO.Cross2CarL := gIO.Cross1CarL;

(*Pedestrians*)
(*If left <-> right cars are going. Up <-> down pedestrians stop. Else go*)
IF gIO.Cross1CarL.GreenLamp OR gIO.Cross1CarL.YellowLamp  THEN
	gIO.Cross1PedLDU.RedLight := TRUE;
	gIO.Cross1PedLDU.GreenLight := FALSE;
	
ELSIF gIO.Cross1CarL.RedLamp THEN
	gIO.Cross1PedLDU.RedLight := FALSE;
	gIO.Cross1PedLDU.GreenLight := TRUE;

END_IF
gIO.Cross1PedLDU.TimeToSequenceChange := UDINT_TO_UINT(vTimeToSequenceChange);

(*Just copy all of the up and downs as they behave the same*)
gIO.Cross1PedLUD := gIO.Cross1PedLDU;
gIO.Cross1PedRUD := gIO.Cross1PedLDU;
gIO.Cross1PedRDU := gIO.Cross1PedLDU;
gIO.Cross2PedLDU := gIO.Cross1PedLDU;
gIO.Cross2PedLUD := gIO.Cross1PedLDU;
gIO.Cross2PedRUD := gIO.Cross1PedLDU;
gIO.Cross2PedRDU := gIO.Cross1PedLDU;

(*If up <-> Down cars are going. Left <-> right pedestrians stop. Else go*)
IF gIO.Cross1CarU.GreenLamp OR gIO.Cross1CarU.YellowLamp THEN
	gIO.Cross1PedULR.RedLight := TRUE;
	gIO.Cross1PedULR.GreenLight := FALSE;

ELSIF gIO.Cross1CarU.RedLamp THEN
	gIO.Cross1PedULR.RedLight := FALSE;
	gIO.Cross1PedULR.GreenLight := TRUE;
END_IF
gIO.Cross1PedULR.TimeToSequenceChange := UDINT_TO_UINT(vTimeToSequenceChange);

(*Just copy all of the lefts and rights as they behave the same*)
gIO.Cross1PedURL := gIO.Cross1PedULR;
gIO.Cross1PedDLR := gIO.Cross1PedULR;
gIO.Cross1PedDRL := gIO.Cross1PedULR;
gIO.Cross2PedULR := gIO.Cross1PedULR;
gIO.Cross2PedURL := gIO.Cross1PedULR;
gIO.Cross2PedDLR := gIO.Cross1PedULR;
gIO.Cross2PedDRL := gIO.Cross1PedULR;
END_PROGRAM
```

## Metrics  

| VAR_IN | VAR_OUT | VAR_IN_OUT | VAR_LOCAL | VAR_EXTERNAL | VAR_GLOBAL | VAR_ACCESS | VAR_TEMP |
| ------ | ------- | ---------- | --------- | ------------ | ---------- | ---------- | -------- |
| 0 | 0 | 0 | 4 | 0 | 0 | 0 | 0 |

| Actions | Lines of code | Maintainable size |
| ------- | ------------- | ----------------- |
| 0 | 101 | 105 |

---
Autogenerated with [ia_tools](https://github.com/tkucic/ia_tools)  

[MAIN]: ../../../../index_st.md
[NAMESPACES]: ../../nsList_st.md
[METRICS]: ../../../metrics_st.md
[BACK]: ../nsMain_st.md
