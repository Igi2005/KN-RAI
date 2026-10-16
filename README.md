# KN-RAI
// ======================
// OPIS ZMIENNYCH DOTYCZACYCH KOSTKI
// ======================

// cube.GET_X
// wspolrzedna X kostki

// cube.GET_Y
// wspolrzedna Y kostki

// cube.GET_Z
// wspolrzedna Z podstawy kostki / powierzchni, na ktorej stoi kostka

// cube.height
// wysokosc kostki

// cube.GET_Z + cube.height
// wysokosc gornej powierzchni kostki

// cube.GET_Z + cube.height + 5
// bezpieczna pozycja chwytaka 5 jednostek nad kostka

// cube.GET_Z + cube.height / 2
// przykladowa wysokosc chwytu w polowie kostki


// ==========================================================
// POCZATEK SEKWENCJI
// ==========================================================

IF START:

    // obliczenie bezpiecznego punktu posredniego
    P1.x := (cube.GET_X + B.x) / 2
    P1.y := (cube.GET_Y + B.y) / 2

    // punkt P1 znajduje sie wyzej od kostki i punktu B
    P1.z := MAX(
        cube.GET_Z + cube.height + 10,
        B.z + 10
    )

    // powrot do pozycji wyjsciowej
    gripper.PTP_HOME := TRUE


IF gripper.PTP_HOME.done:

    // szybki przejazd nad kostke
    gripper.PTP_MOVE(
        cube.GET_X,
        cube.GET_Y,
        cube.GET_Z + cube.height + 5
    )


IF gripper.PTP_MOVE.done:

    gripper.WAIT(2, 's')

    // otwarcie chwytaka
    gripper.OPEN_GRIPPER := TRUE


IF gripper.OPEN_GRIPPER.done:

    // liniowe zejscie do pozycji chwytu
    gripper.LIN_MOVE(
        cube.GET_X,
        cube.GET_Y,
        cube.GET_Z + cube.height / 2
    )


IF gripper.LIN_MOVE.done:

    gripper.WAIT(1, 's')

    // chwycenie kostki
    gripper.CLOSE_GRIPPER := TRUE


IF gripper.CLOSE_GRIPPER.done:

    // pionowe podniesienie kostki
    gripper.LIN_MOVE(
        cube.GET_X,
        cube.GET_Y,
        cube.GET_Z + cube.height + 5
    )


IF gripper.LIN_MOVE.done:

    // przejazd przez bezpieczny punkt posredni
    gripper.LIN_MOVE(P1.x, P1.y, P1.z)


IF gripper.LIN_MOVE.done:

    // dojazd nad punkt B
    gripper.LIN_MOVE(B.x, B.y, B.z + cube.height + 5)


IF gripper.LIN_MOVE.done:

    // liniowe zejscie do miejsca odlozenia
    gripper.LIN_MOVE(
        B.x,
        B.y,
        B.z + cube.height / 2
    )


IF gripper.LIN_MOVE.done:

    // puszczenie kostki
    gripper.OPEN_GRIPPER := TRUE
    gripper.WAIT(1, 's')


IF gripper.OPEN_GRIPPER.done:

    // odejscie pionowo od kostki
    gripper.LIN_MOVE(
        B.x,
        B.y,
        B.z + cube.height + 5
    )


IF gripper.LIN_MOVE.done:

    // powrot przez bezpieczny punkt
    gripper.LIN_MOVE(P1.x, P1.y, P1.z)


IF gripper.LIN_MOVE.done:

    gripper.PTP_HOME := TRUE


IF gripper.PTP_HOME.done:

    STOP
