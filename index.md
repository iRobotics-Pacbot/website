# UIUC Pacbot Competition guidelines

## Next Competition Date

Saturday, April 11 2025

-----

## Pacbot Description

The PacBot competition was started by the Harvard Undergraduate Robotics Club and is now hosted annually at the University of Illinois Urbana-Champaign through iRobotics. The competition takes place in a scaled-up Pacman-inspired arena. The PacBot must navigate the arena and make its trajectory decisions autonomously. Its goal is two-fold: to avoid the pursuing “ghosts” – Inky, Pinky, Blinky, and Clyde – and to collect as many points as possible with only three lives. 

So far, several schools from around North America have taken up the challenge.

## PacBot Rules and Guidelines (Adapted from Harvard, 2022)

**As of 11/2/2025**

Below is the ruleset that must be followed when designing your robots for the third annual PacBot Competition at the University of Illinois Urbana-Champaign. Note that these rules are dynamic and subject to change between now and up to one month before the competition date. Any time the rules are updated, an email will be sent to each participating team alerting them of the change and its purpose. 

Documentation for the game code and PacBot arena, along with example robot designs, can be found at the GitHub: [github.com/HarvardURC/Pacbot](https://github.com/HarvardURC/Pacbot). Teams should consult the GitHub for information regarding communicating with the game from their bots.

-----

## Competition Specifications

### Summary

This competition will mimic the popular game of Pac-Man. It will consist of four “ghosts,” which will be projected onto the arena, and one PacBot, which is the robot each participating team will enter in the competition. Each team’s PacBot must adhere to the specifications outlined below, and the ghosts will follow the specifications provided below. They will navigate in the arena specified below. The objective, much like in the arcade game, is to collect as many “dots” as possible. These are outlined in more detail in the “Arena” section of this document. There will be 4 “special dots” that, upon collection by your PacBot, will enable the PacBot to “eat” the ghosts for extra points. These are outlined in more detail in the “Arena” section of this document. Your PacBot will be given three lives to collect as many of the dots as possible. Scoring is outlined in more detail in the “Gameplay” section of this document. The PacBot with the highest score wins the competition.

### Arena

The competition will consist of an arena, with boundaries 4” tall. The boundaries will be set up to mimic arenas found in the Pacman game. The arena will be identical to the virtual arena used in the game of Pacman (a SolidWorks file of the arena is included in the [GitHub](https://github.com/HarvardURC/Pacbot)), with each passage being 7” wide, NOT including the quarter inch boundary. Account for some tolerance with walls and floor since arenas are hand-constructed by the team. **Teams should be aware that due to design constraints the arena is built in several pieces, and may have small gaps or lips in the seams between the sections.**

Unlike the original Pacman game, PacBots will not be able to treat the map as an infinite continuum. In other words, the PacBots cannot exit the arena on the right side and re-enter on the left side; boundaries will be strict. However, ghosts are only projected onto the field and will be able to exit and enter from opposite sides. 

The arena is programmatically divided into a 28 x 31 grid which is used for reporting the ghosts’ and PacBot’s locations; see [grid.py in the GitHub](https://github.com/HarvardURC/Pacbot/blob/master/src/gameEngine/pacbot/grid.py) for the grid structure. Most grid units will be assumed to have one implicit “dot” that will be automatically collected when the PacBot enters that unit (more details found [here](http://gameinternals.com/post/2072558330/understanding-pac-man-ghost-behavior)). Note that the PacBot does not have to go through the unit to collect the dot; it can enter it and turn around. There will be four “special dots” in the arena at the corresponding locations, as seen in the above webpage. More details about these special dots are given in the “Gameplay” section of this document. The dots on the map will not respawn at any time. 

### Gameplay

The goal of your PacBot is to gain as many points as possible. Ten points are awarded for each white dot consumed by the PacBot. The special dots (“Power Pellets”) are worth 50 points each. When the special dots are taken, a signal will be sent to the ghosts to avoid the PacBot for 20 seconds. At the end of this time, the ghosts will resume chasing the PacBot. In the event that your PacBot reaches another special dot within this time period, the scared timer is reset to 20 seconds for all ghosts. More details on the ghosts’ behavior is outlined in the “Ghosts” section of this document. During this 20 second period in which the ghosts are avoiding the PacBot, your PacBot can pursue the ghosts. If your PacBot catches a ghost, your team will be awarded 200 points for the first ghost, 400 for the second ghost, 800 for the third, and 1600 for the fourth. To catch a ghost, your PacBot must occupy the same space as the ghost. Once a ghost has been eaten, it will respawn in the ghost house and stay there for a short period of time and will no longer be frightened. As in the original game, a bonus cherry will appear (at grid position (13, 13)) when the PacBot has eaten 70 pellets and again at 170 pellets eaten. The cherry disappears after 10 seconds. Eating the bonus cherry awards 100 points.

Any time that a ghost catches your PacBot, your team will be deducted one life. At the end of each life, a “pause” signal will be sent out to your PacBot. Your PacBot **must** stop all autonomous movement at this time. The ghosts will also enter standby mode and will return to their home position. The PacBot will always start each life in its home position (position (14, 7) in arena grid).

The team that has the most points after losing their three lives wins the competition. The dots do not reappear when your PacBot loses a life; upon consumption, the dots vanish for the remainder of the game. The game ends when a team has lost all of their three lives or has collected all of the dots on the board. There are no additional levels beyond the first one. If it should be the case that the 20 second timer from a special dot still has remaining time when all the dots have been consumed, the game will end. Should two or more teams end the game by collecting all of the dots on the board, the winner will first be decided by the number of points, then by the number of lives left, then by the time taken to do so.

All the code that we will run on our arena system (base game code, visualization code and computer vision code) is open source and can be accessed with instructions [here](https://github.com/HarvardURC/Pacbot).

### Ghosts

The ghosts will not take physical form, but will instead be projected onto the arena. Assume that the ghosts will be traveling at 7” per second, which translates to 2 grid units per second, although they may be sped up depending on the performance of the participating PacBots. The ghosts will act independently of each other; they will not assist each other in locating and chasing your PacBot. They will have slight differences in their code to prevent all ghosts from following the same path. These differences will mimic the differences of the code in the actual game of Pacman, found [here](http://gameinternals.com/post/2072558330/understanding-pac-man-ghost-behavior). To catch a ghost, your PacBot must enter the grid space currently occupied by the ghost. Likewise, for a ghost to catch your PacBot, it must also occupy the same grid space simultaneously. When a ghost catches your PacBot, the signal to pause will be sent out. Your PacBot must enter standby mode at this time (read: stop all movement). The PacBot will be placed at its home position, and then the game will be resumed. When your Pacbot catches a ghost, gameplay will continue uninterrupted and the ghost will return to its home position. At the beginning of the game, Blinky will start just outside its home position, and approximately every 5 seconds, another ghost will exit the enclosure. The ghosts are temporarily eliminated when they are caught, and will respawn at their home position.

-----

## Robot Specifications

1. **Each robot must be fully autonomous and automatically respond to signals outlined in this document.** Autonomous means no human intervention in the robot’s functioning. Off robot computation is allowed (i.e. your computer may run code that sends signals to your robot).
2. **There is no budget limit for any team that wishes to participate. You are welcome to spend as much money on your PacBot as you wish provided that it adheres to the specifications below (primarily Specification \#9).**
3. Each robot must be able to completely fit within a 7”x7” grid unit. Bear in mind that this is the minimum distance between walls, so you will likely want to build your robot smaller than this size.
4. Each robot must be no greater than 4” tall.
5. Each PacBot should have a flat area on top with room for a 3” diameter yellow acrylic computer vision target, which will be used for tracking the robot. We will provide these targets at the competition and attach them using tape or velcro.
6. The field will have an overhead camera that the organizers will use to detect the position of the PacBot in the arena grid (see **Arena** above). Each robot will connect to our server via a socket over WiFi to receive the position of the ghosts, and other game state. A library to do this will be provided in Python. **NOTE: We will not be supporting Xbee communication this year.**
    1. The robot will receive messages via a socket that can be read using our library.
    2. When a given function is called, the library will provide an object or struct with the game state to the robot’s code, if an update is available.
    3. If the game state is “paused”, the robot must immediately stop all autonomous movement. As such, the robot must read from the server multiple times a second.
    4. **Detailed documentation on how to work with this code and server can be found at our [GitHub](https://github.com/HarvardURC/Pacbot).**
7. The information sent to the robot will include the following information:
    1. The game state: paused or running
    2. The position of the Pacbot in the arena grid according to the computer vision software
    3. The position of each ghost in the arena grid
    4. Whether each ghost is frightened
    5. The Pacbot’s current score
    6. The Pacbot’s remaining lives
8. Each robot must stay within the boundaries of the arena at all times. Robots cannot damage, adjust, move, or climb over any boundary of the arena (See Specification \#9).
9. Bear in mind that this is a friendly gathering of people seeking to further their knowledge of robots and participate in a fun challenge. For this reason, we urge teams to build their robots in good faith with the spirit of competition. Any unfair or unreasonable characteristic of your PacBot will be considered a violation of this principle.

[Video of Pacbot Demonstration](https://www.youtube.com/watch?v=WoFzPKz9cd4)
