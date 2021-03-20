# Traffic control simulator | [MAIN] | [NAMESPACES] | [METRICS] | [BACK]  

## Documentation for Program simulator  

```pascal
INTERFACE
    VAR
        carC1HR : Sprite; (**)
        carC1HL : Sprite; (**)
        carC1VU : Sprite; (**)
        carC1VD : Sprite; (**)
        carC2VU : Sprite; (**)
        carC2VD : Sprite; (**)
        pedC1VU : Sprite; (**)
        pedC1VD : Sprite; (**)
        pedC2VU : Sprite; (**)
        pedC2VD : Sprite; (**)
        pedC1HL : Sprite; (**)
        pedC2HR : Sprite; (**)
        Cross1CarU : trafficLightCars; (*Lamps*)
        Cross1CarD : trafficLightCars; (**)
        Cross1CarL : trafficLightCars; (**)
        Cross1CarR : trafficLightCars; (**)
        Cross2CarU : trafficLightCars; (**)
        Cross2CarD : trafficLightCars; (**)
        Cross2CarL : trafficLightCars; (**)
        Cross2CarR : trafficLightCars; (**)
        Cross1PedLUD : trafficLightPed; (**)
        Cross1PedLDU : trafficLightPed; (**)
        Cross1PedRUD : trafficLightPed; (**)
        Cross1PedRDU : trafficLightPed; (**)
        Cross1PedULR : trafficLightPed; (**)
        Cross1PedURL : trafficLightPed; (**)
        Cross1PedDLR : trafficLightPed; (**)
        Cross1PedDRL : trafficLightPed; (**)
        Cross2PedLUD : trafficLightPed; (**)
        Cross2PedLDU : trafficLightPed; (**)
        Cross2PedRUD : trafficLightPed; (**)
        Cross2PedRDU : trafficLightPed; (**)
        Cross2PedULR : trafficLightPed; (**)
        Cross2PedURL : trafficLightPed; (**)
        Cross2PedDLR : trafficLightPed; (**)
        Cross2PedDRL : trafficLightPed; (**)
        vCounter : INT; (**)
        vCollision : BOOL; (**)
        vCrashFlagX : INT; (**)
        vCrashFlagY : INT; (**)
    END_VAR
END_INTERFACE
PROGRAM simulator:
    
MOVE_CARS();
MOVE_PEDESTRIANS();

(*Freeze situation if a collission has occured*)
IF NOT vCollision THEN
	UPDATE_OBJECTS();
	CONTROL_LIGHTS();
END_IF
CHECK_COLLISSIONS();



END_PROGRAM
ACTION CHECK_COLLISSIONS:
    (*Check for collisions between each car with another car and pedestrian*)
vCollision := FALSE;
(*Collission car -> car*)
IF isSpriteColliding(carC1HR, carC1HL) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := carC1HL.actY;
END_IF
IF isSpriteColliding(carC1HR, carC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := carC1VU.actY;
END_IF
IF isSpriteColliding(carC1HR, carC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := carC1VD.actY;
END_IF
IF isSpriteColliding(carC1HR, carC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := carC2VD.actY;
END_IF
IF isSpriteColliding(carC1HR, carC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := carC2VU.actY;
END_IF
IF isSpriteColliding(carC1HL, carC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := carC1VU.actY;
END_IF
IF isSpriteColliding(carC1HL, carC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := carC1VD.actY;
END_IF
IF isSpriteColliding(carC1HL, carC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := carC2VU.actY;
END_IF
IF isSpriteColliding(carC1HL, carC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := carC2VD.actY;
END_IF
IF isSpriteColliding(carC1VU, carC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := carC1VD.actY;
END_IF
IF isSpriteColliding(carC1VU, carC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := carC2VD.actY;
END_IF
IF isSpriteColliding(carC1VU, carC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := carC2VU.actY;
END_IF
IF isSpriteColliding(carC1VD, carC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := carC2VD.actY;
END_IF
IF isSpriteColliding(carC1VD, carC2VU) THEN
	vCollision := TRUE; 
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := carC2VU.actY;
END_IF
IF isSpriteColliding(carC2VD, carC2VU) THEN
	vCollision := TRUE; 
	vCrashFlagX := carC2VD.actX;
	vCrashFlagY := carC2VU.actY;
END_IF

(*Check collission car -> pedestrian*)
IF isSpriteColliding(carC1HR, pedC1HL) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF
IF isSpriteColliding(carC1HR, pedC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := pedC1VU.actY;
END_IF
IF isSpriteColliding(carC1HR, pedC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := pedC1VD.actY;
END_IF
IF isSpriteColliding(carC1HR, pedC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := pedC2VU.actY;
END_IF
IF isSpriteColliding(carC1HR, pedC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := pedC2VD.actY;
END_IF
IF isSpriteColliding(carC1HR, pedC2HR) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HR.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF


IF isSpriteColliding(carC1HL, pedC1HL) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF
IF isSpriteColliding(carC1HL, pedC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := pedC1VU.actY;
END_IF
IF isSpriteColliding(carC1HL, pedC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := pedC1VD.actY;
END_IF
IF isSpriteColliding(carC1HL, pedC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := pedC2VU.actY;
END_IF
IF isSpriteColliding(carC1HL, pedC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := pedC2VD.actY;
END_IF
IF isSpriteColliding(carC1HL, pedC2HR) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1HL.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF


IF isSpriteColliding(carC1VU, pedC1HL) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF
IF isSpriteColliding(carC1VU, pedC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := pedC1VU.actY;
END_IF
IF isSpriteColliding(carC1VU, pedC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := pedC1VD.actY;
END_IF
IF isSpriteColliding(carC1VU, pedC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := pedC2VU.actY;
END_IF
IF isSpriteColliding(carC1VU, pedC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := pedC2VD.actY;
END_IF
IF isSpriteColliding(carC1VU, pedC2HR) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VU.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF


IF isSpriteColliding(carC1VD, pedC1HL) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF
IF isSpriteColliding(carC1VD, pedC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := pedC1VU.actY;
END_IF
IF isSpriteColliding(carC1VD, pedC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := pedC1VD.actY;
END_IF
IF isSpriteColliding(carC1VD, pedC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := pedC2VU.actY;
END_IF
IF isSpriteColliding(carC1VD, pedC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := pedC2VD.actY;
END_IF
IF isSpriteColliding(carC1VD, pedC2HR) THEN
	vCollision := TRUE;
	vCrashFlagX := carC1VD.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF


IF isSpriteColliding(carC2VU, pedC1HL) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VU.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF
IF isSpriteColliding(carC2VU, pedC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VU.actX;
	vCrashFlagY := pedC1VU.actY;
END_IF
IF isSpriteColliding(carC2VU, pedC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VU.actX;
	vCrashFlagY := pedC1VD.actY;
END_IF
IF isSpriteColliding(carC2VU, pedC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VU.actX;
	vCrashFlagY := pedC2VU.actY;
END_IF
IF isSpriteColliding(carC2VU, pedC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VU.actX;
	vCrashFlagY := pedC2VD.actY;
END_IF
IF isSpriteColliding(carC2VU, pedC2HR) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VU.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF


IF isSpriteColliding(carC2VD, pedC1HL) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VD.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF
IF isSpriteColliding(carC2VD, pedC1VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VD.actX;
	vCrashFlagY := pedC1VU.actY;
END_IF
IF isSpriteColliding(carC2VD, pedC1VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VD.actX;
	vCrashFlagY := pedC1VD.actY;
END_IF
IF isSpriteColliding(carC2VD, pedC2VU) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VD.actX;
	vCrashFlagY := pedC2VU.actY;
END_IF
IF isSpriteColliding(carC2VD, pedC2VD) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VD.actX;
	vCrashFlagY := pedC2VD.actY;
END_IF
IF isSpriteColliding(carC2VD, pedC2HR) THEN
	vCollision := TRUE;
	vCrashFlagX := carC2VD.actX;
	vCrashFlagY := pedC1HL.actY;
END_IF

(*Hide the crash flag if there is no collission*)
IF NOT vCollision THEN
	vCrashFlagX := -200;
	vCrashFlagY := -200;
END_IF

END_ACTION
ACTION MOVE_CARS:
    (*Car C1 HL. Moves from left to right, stops at the crossroads and moves if only green is lit*)
IF carC1HL.actX = -49 THEN
	IF (Cross1CarL.GreenLamp AND NOT (Cross1CarL.RedLamp OR Cross1CarL.YellowLamp)) THEN
		carC1HL.moveX := 1;
	ELSE
		carC1HL.moveX := 0;
	END_IF
ELSIF carC1HL.actX = 1211 THEN
	IF (Cross2CarL.GreenLamp AND NOT (Cross2CarL.RedLamp OR Cross2CarL.YellowLamp)) THEN
		carC1HL.moveX := 1;
	ELSE
		carC1HL.moveX := 0;
	END_IF
ELSE
	carC1HL.moveX := 1;
END_IF

(*Car C1 HR. Moves from right to left, stops at the crossroads and moves if only green is lit*)
IF carC1HR.actX = 567 THEN
	IF (Cross1CarR.GreenLamp AND NOT (Cross1CarR.RedLamp OR Cross1CarR.YellowLamp)) THEN
		carC1HR.moveX := -1;
	ELSE
		carC1HR.moveX := 0;
	END_IF
ELSIF carC1HR.actX = 1829 THEN
	IF (Cross2CarR.GreenLamp AND NOT (Cross2CarR.RedLamp OR Cross2CarR.YellowLamp)) THEN
		carC1HR.moveX := -1;
	ELSE
		carC1HR.moveX := 0;
	END_IF
ELSE
	carC1HR.moveX := -1;
END_IF
(*Car C1 VU. Moves from up to down, stops at the crossroads and moves if only green is lit*)
IF carC1VU.actY = -49 THEN
	IF (Cross1CarU.GreenLamp AND NOT (Cross1CarU.RedLamp OR Cross1CarU.YellowLamp)) THEN
		carC1VU.moveY := 1;
	ELSE
		carC1VU.moveY := 0;
	END_IF
ELSE
	carC1VU.moveY := 1;
END_IF
(*Car C2 VU. Moves from up to down, stops at the crossroads and moves if only green is lit*)
IF carC2VU.actY = -49 THEN
	IF (Cross2CarU.GreenLamp AND NOT (Cross2CarU.RedLamp OR Cross2CarU.YellowLamp)) THEN
		carC2VU.moveY := 1;
	ELSE
		carC2VU.moveY := 0;
	END_IF
ELSE
	carC2VU.moveY := 1;
END_IF
(*Car C1 VD. Moves from down to up, stops at the crossroads and moves if only green is lit*)
IF carC1VD.actY = 564 THEN
	IF (Cross1CarD.GreenLamp AND NOT (Cross1CarD.RedLamp OR Cross1CarD.YellowLamp)) THEN
		carC1VD.moveY := -1;
	ELSE
		carC1VD.moveY := 0;
	END_IF
ELSE
	carC1VD.moveY := -1;
END_IF
(*Car C2 VD. Moves from down to up, stops at the crossroads and moves if only green is lit*)
IF carC2VD.actY = 564 THEN
	IF (Cross2CarD.GreenLamp AND NOT (Cross2CarD.RedLamp OR Cross2CarD.YellowLamp)) THEN
		carC2VD.moveY := -1;
	ELSE
		carC2VD.moveY := 0;
	END_IF
ELSE
	carC2VD.moveY := -1;
END_IF
END_ACTION
ACTION MOVE_PEDESTRIANS:
    (*Pedestrian C1 VU. Moves from up to down, stops at the crossroads and moves if only green is lit*)
IF pedC1VU.actY = 180 THEN
	IF Cross1PedLUD.GreenLight AND NOT Cross1PedLUD.RedLight THEN
		pedC1VU.moveY := 1;
	ELSE
		pedC1VU.moveY := 0;
	END_IF
ELSE
	pedC1VU.moveY := 1;
END_IF
(*Pedestrian C2 VU. Moves from up to down, stops at the crossroads and moves if only green is lit*)
IF pedC2VU.actY = 180 THEN
	IF Cross2PedLUD.GreenLight AND NOT Cross2PedLUD.RedLight THEN
		pedC2VU.moveY := 1;
	ELSE
		pedC2VU.moveY := 0;
	END_IF
ELSE
	pedC2VU.moveY := 1;
END_IF
(*Pedestrian C1 VD. Moves from down to up, stops at the crossroads and moves if only green is lit*)
IF pedC1VD.actY = 480 THEN
	IF Cross1PedLDU.GreenLight AND NOT Cross1PedLDU.RedLight THEN
		pedC1VD.moveY := -1;
	ELSE
		pedC1VD.moveY := 0;
	END_IF
ELSE
	pedC1VD.moveY := -1;
END_IF
(*Pedestrian C1 VD. Moves from down to up, stops at the crossroads and moves if only green is lit*)
IF pedC2VD.actY = 480 THEN
	IF Cross2PedLDU.GreenLight AND NOT Cross2PedLDU.RedLight THEN
		pedC2VD.moveY := -1;
	ELSE
		pedC2VD.moveY := 0;
	END_IF
ELSE
	pedC2VD.moveY := -1;
END_IF
(*Pedestrian C1 HL. Moves from left to right, stops at the crossroads and moves if only green is lit*)
IF pedC1HL.actX = 190 THEN
	IF Cross1PedDLR.GreenLight AND NOT Cross1PedDLR.RedLight THEN
		pedC1HL.moveX := 1;
	ELSE
		pedC1HL.moveX := 0;
	END_IF
ELSIF pedC1HL.actX = 1450 THEN
	IF Cross2PedDLR.GreenLight AND NOT Cross2PedDLR.RedLight THEN
		pedC1HL.moveX := 1;
	ELSE
		pedC1HL.moveX := 0;
	END_IF
ELSE
	pedC1HL.moveX := 1;
END_IF
(*Pedestrian C1 HR. Moves from right to left, stops at the crossroads and moves if only green is lit*)
IF pedC2HR.actX = 1740 THEN
	IF Cross2PedURL.GreenLight AND NOT Cross2PedURL.RedLight THEN
		pedC2HR.moveX := -1;
	ELSE
		pedC2HR.moveX := 0;
	END_IF
ELSIF pedC2HR.actX = 480 THEN
	IF Cross1PedURL.GreenLight AND NOT Cross1PedURL.RedLight THEN
		pedC2HR.moveX := -1;
	ELSE
		pedC2HR.moveX := 0;
	END_IF
ELSE
	pedC2HR.moveX := -1;
END_IF
END_ACTION
ACTION UPDATE_OBJECTS:
    // Update sprites
carC1HR();
carC1HL();
carC1VD();
carC1VU();
carC2VD();
carC2VU();
pedC1VU();
pedC1VD();
pedC2VU();
pedC2VD();
pedC1HL();
pedC2HR();
END_ACTION
ACTION CONTROL_LIGHTS:
    Cross1CarU := gIO.Cross1CarU;
Cross1CarD := gIO.Cross1CarD;
Cross1CarL := gIO.Cross1CarL;
Cross1CarR := gIO.Cross1CarR;

Cross2CarU := gIO.Cross2CarU;
Cross2CarD := gIO.Cross2CarD;
Cross2CarL := gIO.Cross2CarL;
Cross2CarR := gIO.Cross2CarR;

Cross1PedLUD := gIO.Cross1PedLUD;
Cross1PedLDU := gIO.Cross1PedLDU;
Cross1PedRUD := gIO.Cross1PedRUD;
Cross1PedRDU := gIO.Cross1PedRDU;

Cross1PedULR := gIO.Cross1PedULR;
Cross1PedURL := gIO.Cross1PedURL;
Cross1PedDLR := gIO.Cross1PedDLR;
Cross1PedDRL := gIO.Cross1PedDRL;

Cross2PedLUD := gIO.Cross2PedLUD;
Cross2PedLDU := gIO.Cross2PedLDU;
Cross2PedRUD := gIO.Cross2PedRUD;
Cross2PedRDU := gIO.Cross2PedRDU;

Cross2PedULR := gIO.Cross2PedULR;
Cross2PedURL := gIO.Cross2PedURL;
Cross2PedDLR := gIO.Cross2PedDLR;
Cross2PedDRL := gIO.Cross2PedDRL;
END_ACTION
```

## Metrics  

| VAR_IN | VAR_OUT | VAR_IN_OUT | VAR_LOCAL | VAR_EXTERNAL | VAR_GLOBAL | VAR_ACCESS | VAR_TEMP | Actions | Lines of code | Maintainable size |
| ------ | ------- | ---------- | --------- | ------------ | ---------- | ---------- | -------- | ------- | ------------- | ----------------- |
| 0 | 0 | 0 | 40 | 0 | 0 | 0 | 0 | 5 | 475 | 515 |  

---
Autogenerated with [ia_tools](https://github.com/tkucic/ia_tools)  

[MAIN]: ../../../../index_st.md
[NAMESPACES]: ../../nsList_st.md
[METRICS]: ../../../metrics_st.md
[BACK]: ../nsMain_st.md
