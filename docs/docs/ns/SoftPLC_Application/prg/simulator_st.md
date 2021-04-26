# Traffic control simulator | [MAIN] | [NAMESPACES] | [METRICS] | [BACK]  

## Documentation for Program simulator  

```pascal
INTERFACE
    VAR 
        carC1HR : Sprite; (*Sprite controling car C1HR*)
        carC1HL : Sprite; (*Sprite controling car C1HL*)
        carC1VU : Sprite; (*Sprite controling car C1VU*)
        carC1VD : Sprite; (*Sprite controling car C1VD*)
        carC2VU : Sprite; (*Sprite controling car C2VU*)
        carC2VD : Sprite; (*Sprite controling car C2VD*)
        pedC1VU : Sprite; (*Sprite controling pedestrian C1VU*)
        pedC1VD : Sprite; (*Sprite controling pedestrian C1VD*)
        pedC2VU : Sprite; (*Sprite controling pedestrian C2VU*)
        pedC2VD : Sprite; (*Sprite controling pedestrian C2VD*)
        pedC1HL : Sprite; (*Sprite controling pedestrian C1HL*)
        pedC2HR : Sprite; (*Sprite controling pedestrian C1HR*)
        Cross1CarU : trafficLightCars; (*Car traffic light - Crossroad 1 - Cars going up to down*)
        Cross1CarD : trafficLightCars; (*Car traffic light - Crossroad 1 - Cars going down to up*)
        Cross1CarL : trafficLightCars; (*Car traffic light - Crossroad 1 - Cars going left to right*)
        Cross1CarR : trafficLightCars; (*Car traffic light - Crossroad 1 - Cars going right to left*)
        Cross2CarU : trafficLightCars; (*Car light - Crossroad 2 - Cars going up to down*)
        Cross2CarD : trafficLightCars; (*Car light - Crossroad 2 - Cars going down to up*)
        Cross2CarL : trafficLightCars; (*Car light - Crossroad 2 - Cars going left to right*)
        Cross2CarR : trafficLightCars; (*Car light - Crossroad 2 - Cars going right to left*)
        Cross1PedLUD : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going up to down on left side*)
        Cross1PedLDU : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going down to up on left side*)
        Cross1PedRUD : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going up to down on right side*)
        Cross1PedRDU : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going down to up on right side*)
        Cross1PedULR : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going left to right on upper side*)
        Cross1PedURL : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going right to left on upper side*)
        Cross1PedDLR : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going left to right on lower side*)
        Cross1PedDRL : trafficLightPed; (*Pedestrian light - Crossroad 1 - Pedestrians going right to left on lower side*)
        Cross2PedLUD : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going up to down on left side*)
        Cross2PedLDU : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going down to up on left side*)
        Cross2PedRUD : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going up to down on right side*)
        Cross2PedRDU : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going down to up on right side*)
        Cross2PedULR : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going left to right on upper side*)
        Cross2PedURL : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going right to left on upper side*)
        Cross2PedDLR : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going left to right on lower side*)
        Cross2PedDRL : trafficLightPed; (*Pedestrian light - Crossroad 2 - Pedestrians going right to left on lower side*)
        vCollision : BOOL; (*Collission has occured *)
        vDangerousCombination : BOOL; (*Dangerous combination of lights has been detected *)
        vPoliceControl : BOOL; (*Police control activated *)
        vCrashFlagX : INT := -200; (*If collission is detected, holds position of collission x axis *)
        vCrashFlagY : INT := -200; (*If collission is detected, holds position of collission y axis *)
        Blinker : BLINK; (*Code that blinks a variable*)
        vFaultReason : STRING[255]; (*If collission or dangerous combination detected, holds more information where it has occured*)
    END_VAR
END_INTERFACE
PROGRAM simulator:
    MOVE_CARS();
    MOVE_PEDESTRIANS();

    (*Blink yellow signal if the police is under control*)
    IF vPoliceControl THEN
    	UPDATE_OBJECTS();
    	BLINK_YELLOW_SIGNAL();
    (*Freeze situation if a collission or dangerous situation has occured*)
    ELSIF vCollision OR vDangerousCombination THEN
    	BLINK_DANGER_SIGNAL();
    ELSE
    	(*Everything is fine so move objects and let the main control the lights*)
    	UPDATE_OBJECTS();
    	CONTROL_LIGHTS();
    	vFaultReason := '';
    END_IF
    FAULT_MONITOR();
    gIO.PoliceModeActivated := vPoliceControl;
    Blinker(ENABLE:=TRUE, TIMELOW:=T#1S, TIMEHIGH:=T#1S);



END_PROGRAM
ACTION CHECK_COLLISSIONS:
    (*Check for collisions between each car with another car and pedestrian*)
    vCollision := FALSE;

    (*Collission car -> car*)
    IF isSpriteColliding(carC1HR, carC1HL) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := carC1HL.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND CAR C1HL';
    END_IF
    IF isSpriteColliding(carC1HR, carC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := carC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND CAR C1VU';
    END_IF
    IF isSpriteColliding(carC1HR, carC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := carC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND CAR C1VD';
    END_IF
    IF isSpriteColliding(carC1HR, carC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := carC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND CAR C2VD';
    END_IF
    IF isSpriteColliding(carC1HR, carC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := carC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND CAR C2VU';
    END_IF
    IF isSpriteColliding(carC1HL, carC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := carC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND CAR C1VU';
    END_IF
    IF isSpriteColliding(carC1HL, carC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := carC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND CAR C1VD';
    END_IF
    IF isSpriteColliding(carC1HL, carC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := carC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND CAR C2VU';
    END_IF
    IF isSpriteColliding(carC1HL, carC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := carC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND CAR C2VD';
    END_IF
    IF isSpriteColliding(carC1VU, carC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := carC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND CAR C1VD';
    END_IF
    IF isSpriteColliding(carC1VU, carC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := carC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND CAR C2VD';
    END_IF
    IF isSpriteColliding(carC1VU, carC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := carC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND CAR C2VU';
    END_IF
    IF isSpriteColliding(carC1VD, carC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := carC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND CAR C2VD';
    END_IF
    IF isSpriteColliding(carC1VD, carC2VU) THEN
    	vCollision := TRUE; 
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := carC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND CAR C2VU';
    END_IF
    IF isSpriteColliding(carC2VD, carC2VU) THEN
    	vCollision := TRUE; 
    	vCrashFlagX := carC2VD.actX;
    	vCrashFlagY := carC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VD AND CAR C2VU';
    END_IF

    (*Check collission car -> pedestrian*)
    IF isSpriteColliding(carC1HR, pedC1HL) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := pedC1HL.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND PEDESTRIAN C1HL';
    END_IF
    IF isSpriteColliding(carC1HR, pedC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := pedC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND PEDESTRIAN C1VU';
    END_IF
    IF isSpriteColliding(carC1HR, pedC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := pedC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND PEDESTRIAN C1VD';
    END_IF
    IF isSpriteColliding(carC1HR, pedC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := pedC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND PEDESTRIAN C2VU';
    END_IF
    IF isSpriteColliding(carC1HR, pedC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := pedC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND PEDESTRIAN C2VD';
    END_IF
    IF isSpriteColliding(carC1HR, pedC2HR) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HR.actX;
    	vCrashFlagY := pedC2HR.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HR AND PEDESTRIAN C2HR';
    END_IF


    IF isSpriteColliding(carC1HL, pedC1HL) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := pedC1HL.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND PEDESTRIAN C1HL';
    END_IF
    IF isSpriteColliding(carC1HL, pedC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := pedC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND PEDESTRIAN C1VU';
    END_IF
    IF isSpriteColliding(carC1HL, pedC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := pedC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND PEDESTRIAN C1VD';
    END_IF
    IF isSpriteColliding(carC1HL, pedC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := pedC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND PEDESTRIAN C2VU';
    END_IF
    IF isSpriteColliding(carC1HL, pedC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := pedC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND PEDESTRIAN C2VD';
    END_IF
    IF isSpriteColliding(carC1HL, pedC2HR) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1HL.actX;
    	vCrashFlagY := pedC2HR.actY;
    	vFaultReason := 'COLLISSION AT CAR C1HL AND PEDESTRIAN C2HR';
    END_IF


    IF isSpriteColliding(carC1VU, pedC1HL) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := pedC1HL.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND PEDESTRIAN C1HL';
    END_IF
    IF isSpriteColliding(carC1VU, pedC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := pedC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND PEDESTRIAN C1VU';
    END_IF
    IF isSpriteColliding(carC1VU, pedC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := pedC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND PEDESTRIAN C1VD';
    END_IF
    IF isSpriteColliding(carC1VU, pedC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := pedC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND PEDESTRIAN C2VU';
    END_IF
    IF isSpriteColliding(carC1VU, pedC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := pedC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND PEDESTRIAN C2VD';
    END_IF
    IF isSpriteColliding(carC1VU, pedC2HR) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VU.actX;
    	vCrashFlagY := pedC2HR.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VU AND PEDESTRIAN C2HR';
    END_IF


    IF isSpriteColliding(carC1VD, pedC1HL) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := pedC1HL.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND PEDESTRIAN C1HL';
    END_IF
    IF isSpriteColliding(carC1VD, pedC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := pedC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND PEDESTRIAN C1VU';
    END_IF
    IF isSpriteColliding(carC1VD, pedC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := pedC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND PEDESTRIAN C1VD';
    END_IF
    IF isSpriteColliding(carC1VD, pedC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := pedC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND PEDESTRIAN C2VU';
    END_IF
    IF isSpriteColliding(carC1VD, pedC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := pedC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND PEDESTRIAN C2VD';
    END_IF
    IF isSpriteColliding(carC1VD, pedC2HR) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC1VD.actX;
    	vCrashFlagY := pedC2HR.actY;
    	vFaultReason := 'COLLISSION AT CAR C1VD AND PEDESTRIAN C2HR';
    END_IF


    IF isSpriteColliding(carC2VU, pedC1HL) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VU.actX;
    	vCrashFlagY := pedC1HL.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VU AND PEDESTRIAN C1HL';
    END_IF
    IF isSpriteColliding(carC2VU, pedC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VU.actX;
    	vCrashFlagY := pedC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VU AND PEDESTRIAN C1VU';
    END_IF
    IF isSpriteColliding(carC2VU, pedC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VU.actX;
    	vCrashFlagY := pedC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VU AND PEDESTRIAN C1VD';
    END_IF
    IF isSpriteColliding(carC2VU, pedC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VU.actX;
    	vCrashFlagY := pedC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VU AND PEDESTRIAN C2VU';
    END_IF
    IF isSpriteColliding(carC2VU, pedC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VU.actX;
    	vCrashFlagY := pedC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VU AND PEDESTRIAN C2VD';
    END_IF
    IF isSpriteColliding(carC2VU, pedC2HR) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VU.actX;
    	vCrashFlagY := pedC2HR.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VU AND PEDESTRIAN C2HR';
    END_IF


    IF isSpriteColliding(carC2VD, pedC1HL) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VD.actX;
    	vCrashFlagY := pedC1HL.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VD AND PEDESTRIAN C1HL';
    END_IF
    IF isSpriteColliding(carC2VD, pedC1VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VD.actX;
    	vCrashFlagY := pedC1VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VD AND PEDESTRIAN pedC1VU';
    END_IF
    IF isSpriteColliding(carC2VD, pedC1VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VD.actX;
    	vCrashFlagY := pedC1VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VD AND PEDESTRIAN C1VD';
    END_IF
    IF isSpriteColliding(carC2VD, pedC2VU) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VD.actX;
    	vCrashFlagY := pedC2VU.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VD AND PEDESTRIAN C2VU';
    END_IF
    IF isSpriteColliding(carC2VD, pedC2VD) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VD.actX;
    	vCrashFlagY := pedC2VD.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VD AND PEDESTRIAN C2VD';
    END_IF
    IF isSpriteColliding(carC2VD, pedC2HR) THEN
    	vCollision := TRUE;
    	vCrashFlagX := carC2VD.actX;
    	vCrashFlagY := pedC2HR.actY;
    	vFaultReason := 'COLLISSION AT CAR C2VD AND PEDESTRIAN C2HR';
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
ACTION FAULT_MONITOR:
    (*Checks if dangerous situations occur, horizontal wave and vertical wave green at the same time*)
    vDangerousCombination := FALSE;
    (*Green vertical and horizontal pedestrians green*)
    IF Cross1CarU.GreenLamp AND (Cross1PedULR.GreenLight OR Cross1PedURL.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS1 -> VERTICAL CARS AND HORIZONTAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF
    IF Cross2CarU.GreenLamp AND (Cross2PedULR.GreenLight OR Cross2PedURL.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS2 -> VERTICAL CARS AND HORIZONTAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF
    IF Cross1CarD.GreenLamp AND (Cross1PedDLR.GreenLight OR Cross1PedDRL.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS1 -> VERTICAL CARS AND HORIZONTAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF
    IF Cross2CarD.GreenLamp AND (Cross2PedDLR.GreenLight OR Cross2PedDRL.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS2 -> VERTICAL CARS AND HORIZONTAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF

    (*Green horizontal and vertical pedestrians green*)
    IF Cross1CarL.GreenLamp AND (Cross1PedLUD.GreenLight OR Cross1PedLDU.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS1 -> HORIZONTAL CARS AND VERTICAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF
    IF Cross1CarR.GreenLamp AND (Cross1PedRUD.GreenLight OR Cross1PedRDU.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS1 -> HORIZONTAL CARS AND VERTICAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF
    IF Cross2CarL.GreenLamp AND (Cross2PedLUD.GreenLight OR Cross2PedLDU.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS2 -> HORIZONTAL CARS AND VERTICAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF
    IF Cross2CarR.GreenLamp AND (Cross2PedRUD.GreenLight OR Cross2PedRDU.GreenLight) THEN
    	vDangerousCombination := TRUE;
    	vFaultReason := 'DANGEROUS COMBINATION AT CROSS2 -> HORIZONTAL CARS AND VERTICAL PEDESTRIANS GREEN AT THE SAME TIME';
    END_IF

    (*Checks if there was a collission with car-car and ped - car*)
    CHECK_COLLISSIONS();
END_ACTION
ACTION BLINK_DANGER_SIGNAL:
    Cross1CarU.GreenLamp := FALSE;
    Cross1CarU.YellowLamp := FALSE;
    Cross1CarU.RedLamp := Blinker.OUT;

    Cross1CarD := Cross1CarU;
    Cross1CarL := Cross1CarU;
    Cross1CarR := Cross1CarU;

    Cross2CarU := Cross1CarU;
    Cross2CarD := Cross1CarU;
    Cross2CarL := Cross1CarU;
    Cross2CarR := Cross1CarU;

    Cross1PedLUD.GreenLight := FALSE;
    Cross1PedLUD.RedLight := Blinker.OUT;
    Cross1PedLUD.TimeToSequenceChange := 0;

    Cross1PedLDU := Cross1PedLUD;
    Cross1PedRUD := Cross1PedLUD;
    Cross1PedRDU := Cross1PedLUD;

    Cross1PedULR := Cross1PedLUD;
    Cross1PedURL := Cross1PedLUD;
    Cross1PedDLR := Cross1PedLUD;
    Cross1PedDRL := Cross1PedLUD;

    Cross2PedLUD := Cross1PedLUD;
    Cross2PedLDU := Cross1PedLUD;
    Cross2PedRUD := Cross1PedLUD;
    Cross2PedRDU := Cross1PedLUD;

    Cross2PedULR := Cross1PedLUD;
    Cross2PedURL := Cross1PedLUD;
    Cross2PedDLR := Cross1PedLUD;
    Cross2PedDRL := Cross1PedLUD;
END_ACTION
ACTION BLINK_YELLOW_SIGNAL:
    Cross1CarU.GreenLamp := FALSE;
    Cross1CarU.YellowLamp := Blinker.OUT;
    Cross1CarU.RedLamp := FALSE;

    Cross1CarD := Cross1CarU;
    Cross1CarL := Cross1CarU;
    Cross1CarR := Cross1CarU;

    Cross2CarU := Cross1CarU;
    Cross2CarD := Cross1CarU;
    Cross2CarL := Cross1CarU;
    Cross2CarR := Cross1CarU;

    Cross1PedLUD.GreenLight := FALSE;
    Cross1PedLUD.RedLight := FALSE;
    Cross1PedLUD.TimeToSequenceChange := 0;

    Cross1PedLDU := Cross1PedLUD;
    Cross1PedRUD := Cross1PedLUD;
    Cross1PedRDU := Cross1PedLUD;

    Cross1PedULR := Cross1PedLUD;
    Cross1PedURL := Cross1PedLUD;
    Cross1PedDLR := Cross1PedLUD;
    Cross1PedDRL := Cross1PedLUD;

    Cross2PedLUD := Cross1PedLUD;
    Cross2PedLDU := Cross1PedLUD;
    Cross2PedRUD := Cross1PedLUD;
    Cross2PedRDU := Cross1PedLUD;

    Cross2PedULR := Cross1PedLUD;
    Cross2PedURL := Cross1PedLUD;
    Cross2PedDLR := Cross1PedLUD;
    Cross2PedDRL := Cross1PedLUD;
END_ACTION
```

## Metrics  

- VAR : 43

| Actions | Lines of code | Maintainable size |
| ------- | ------------- | ----------------- |
| 8 | 646 | 689 |

---
Autogenerated with [ia_tools](https://github.com/tkucic/ia_tools)  

[MAIN]: ../../../../index_st.md
[NAMESPACES]: ../../nsList_st.md
[METRICS]: ../../../metrics_st.md
[BACK]: ../nsMain_st.md
