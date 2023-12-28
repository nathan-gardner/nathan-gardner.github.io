---
layout: project
title: rpsls
---

### Rock Paper Scissors Lizard Spock for Introduction to Digital Systems

![](./../images/RSPLS/rspls.jpg)

Rock Paper Scissors Lizard Spock is a spin-off of the classic game Rock Paper Scissors that was popularized by the TV sitcom The Big Bang Theory. RPSLS adds onto Rock Paper Scissors by adding two objects: Lizard (which beats Rock and Paper but loses to Scissors and Spock) and Spock (which defeats Scissors and Rock but is conquered by Paper and Lizard).

This game console was designed to simulate a best-of-three RPSLS competition between two players, in which the game will continue until one player has reached a score of two. Each player has five object choice buttons to select from. Each player possesses a “battle” button, and to initiate a “battle” both must simultaneously press their battle buttons along with their selected object choice button. Players are instructed to press only one object choice button at a time. However, if a battle is initiated with a player pressing multiple input buttons at the same time, priority of the inputs is given in the following order from highest to lowest: Spock, Scissors, Rock, Paper, and Lizard. If both players simultaneously press their battle buttons but either of them fails to press an object choice button at that moment, that player’s selection is defaulted to the lowest priority object Lizard.

The game console has four outputs: two LEDs and two seven segment displays, both of which have their own finite state machine (FSM). The LED indicates which player has won after each turn and the seven segment represents the score for the current game.

![](./../images/RSPLS/rspls.jpg)

![](./../images/RSPLS/priorityencoder.png) | ![](./../images/RSPLS/scoreboardfsm.png)                              
:-----------------------------------------:|:-----------------------------------------:
![](./../images/RSPLS/wincomp.png)         |  ![](./../images/RSPLS/LEDfsm.png)
