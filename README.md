# New-World-CreatureInfo
Similar to the minimap application, this program gets information from the center of the screen and displays information about that creature from a database.

# Download
[Latest Download Page](https://github.com/llabinator/New-World-CreatureInfo/releases/latest)

# The Program
A creature info tool for New World. The application takes screenshots of your Primary Monitor Screen using Optical Character Recognition (OCR) to detect creatures you are looking at. It will then display the creatures health, and xp gained when killed (variable depending on your own level), depending on their level. The application DOES NOT interact with the game itself. It DOES NOT inject itself into memory. It DOES NOT read game memory. It DOES NOT contact with the game whatsoever. The program simply takes the information being displayed on your screen. It is no different than recording/streaming your screen, therefore, it applys to the terms and service (CONFIRMED BY AGS THEMSELVES).

# Must Haves
You must have Java installed on your PC. Java version 1.8+. You must also have Firefox installed on your PC. This is because I am using Seleniums firefox drivers.

# Discord
Join the [discord](https://discord.gg/GdhEdD9umc) if you have any questions, want to contribute, or have any problems with the program.

# What To Do
- Download the .rar from the [Download Page](https://github.com/llabinator/New-World-CreatureInfo/releases/latest)
- Extract the .rar
- To Run with Non-Console mode for the application just open the .jar with "Java(TM) Platform SE Binary"
  
- To Run with Console mode (mainly for debugging), open cmd and change directory into the un-zipped folder
  - Then type: java -jar CreatureInfo.jar

- A small window should appear in the top left, feel free to move it and resize it however you want. This is where the creature information will be displayed.

- You should be able to look at a creature/mob for a second or two, and it update in the top left with information for all creatures/mobs of that type stored in the database. Sometimes it takes longer depending on the surroundings (Gray surroundings make it hard for the OCR to see the nametags).

**TURN ON "Bigger' TEXT IN Settings > Accessibility > Text Size for better results!**

**Program might freeze and need restarting... Potential Memory Leak - Working on a fix**

**NOTE: Not all creatures are put into the database; therefore, some creatures will not work when hovering over them. Also, a lot of creatures will just have shortened names. For example, 'Peppered Lynx' does not exist in the databse, but it will still find 'Lynx' which should give similar information.**
**There is also a 5 second break after it has found a creature, in order to reduce usage... This is subject to change.**

- *If errors persist, please contact me at my [discord](https://discord.gg/HxsTVM3wB2)*

**NOTE THIS WARNING IS NORMAL (console mode only): Warning: Invalid resolution 0 dpi. Using 70 instead. I can't seem to find a way around it not yelling this constantly.**

# Licensing
This program is fully open-source under the MIT license. Thanks to ITesseract for allowing me to create this application, the licenses for the methods used are in the Licenses folder.
