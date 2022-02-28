# cs50_final_project : Unity Game: "Memorize what is -NOT- real"

#### Video Demo:  <URL HERE>

You can download the project here: https://www.dropbox.com/s/p8ggqjcmtcu9kpu/Memo_FinalBuild_270222.zip?dl=0
Alternative link: https://mcagnus1337.itch.io/memorize-what-is-not-real

#### Description: Hello all, for my final project I was going to do a Unity game and did this in parallel to Brackeys Game Jam 2022.1. The theme of the Game Jam was "It is not real". Therefore I made a memory like game, where you have to identify each round, if there has been added a new object to the board. The game jam did last from 20. Feb 2022 until 27. Feb 2022. I did work around 8-12 hours each day on it. Next to the programming part I also have done the pixel art by myself in the project, but my overall focus was on the programming part for this Game Jam.
  
I have a lot of scripts inside of my project. I will focus on the Scripts only here written in C#:
  
  1. GameManager.cs
  The GameManager.cs script is one of the first scripts I have implemented. My game is seperated into different Game States, which are being declared in this script. I also declared this on to being a singelton so I can access an instance of this script basically from everywhere in my project very easy. It defines the stages of my Game. Main Menu, Build Phase, LookPhase, SleepPhase, SelectionPhase, CheckPhase and EndPhase. Pretty easy script overall but the most important part taking care of what stage is active at the current moment. Here I am using events. Other scripts can call the method UpdateGameState and for each Game State change it will invoke an event. Everyone who is listening to this event is then acting independent of this script.
  
  2. HandleMainMenuPhase.cs
  Listens to the previous mentioned UpdateGameState event. If GameState == MainMenu it will reset the board. The script also offers a lot of methods for the buttons in the main menu. like "SetEasy(),SetMedium(),SetHard(),SetCustom(),StartGame(),ExitGame() and ResetBoard().
  
  3. DeleteObject.cs
The DeleteObject.cs is attached to each GameObject, which is going to be placed on the board. Thats basically all(most) of the objects since I am individually creating the board based on what the player wants to have (See custom mode ingame). The ResetBoard() method in "HandleMainMenuPhase.cs" will search for all objects having this "DeleteObject.cs" attached and call the DeleteSelf() method to destroy itself. Very simple script. 3 LOC.
  
  4. HandleBuildPhase.cs
Pressing start in (2 - HandleMainMenuPhase) the BuildPhase will triggered and the HandleBuildPhase.cs is listening to this event being invoked in (1 - GameManager).
In recap perspective this script is not really used for anything to be honest, since I am doing all the invididual board building staff in other scripts...But it will call the "LookPhase" to "start" with the game (from the perspective of the player).
  
  5. CreateSpawnPoints.cs
 This script is the main script to build individual game boards, based on level or custom data the player is handing over via UI. Listens to (1 -GameManager.cs) event OnGameStateChange.
  It places Inner and Outter Tiles of the board and also creates the SpawnPoints, where later objects can spawn on the board. How big the board will be will be defined in a independent "Scriptable Object", which is in this case just a data container to store values. The values change depending on which level the player wants to play.
  
  6. SpawnLocationManager.cs
  Super simple script. 1 real LOC. Containing a bool value isFree. This script is attached to the SpawnPoints (so called Prefabs) to later check if the SpawnPoints are still free or not. If free an object will spawn there (potentially).
  
  7. HandleLookPhase.cs
  This script handles the LookPhase. LookPhase is very simple. In the beginning of the game you will get a few seconds to get an overview of the newly generated board. It contains of a timer. If timer is 0 or below it will Update (1 GameManager) to be in SleepPhase.
  
  8. HandleSleepPhase.cs
  This script is also listening to the event in (1 GameManager.cs). If in SleepPhase it will Start a Timer (another script) on how long this SleepPhase will last.
  After the timer ended (also has a event this script is listening to) this script will handle Object Spawns as well as potential Enemy and Boss spawn.
  After this is done SelectionPhase Game state will be set.
  
  9. TimerBehavior.cs
  Simple timer script. It contains a StartTimer(float time) method which is called from outside. If timer ended it will also invoke an event. All listeners will then be contacted and act. The script is also attached to an image fill for the Timer UI. It handles the "fill" of the image based on how much % of time is over. This script is getting called in SleepPhase but also in SelectionPhase, which I am describing later.
  
  10. Spawner.cs
  This script takes care of the object Spawn. It contains an area of all GameObjects (Prefabs) who should spawn on the board during SleepPhase(or are already on board initaially) and spawns the objects on the board. Therefore it identifies all SpawnPoints (5) and checks if it is free in the (6) SpawnLocationManager. Therefore it counts up an object ID, so the programm is able to identify which object has spawned when..
  
  11. ObjectID.cs
  Super simple script. Just contains an INT value. Each Object spawned holds this script. Therefore it can be identified. 
  
  12. HandleSelectionPhase.cs
  Also listens to (9) Timer Behavior. If Time is up, the SelectionPhase ends. (The player only has a limited amount of time to choose the newly spawned object) Also in the script it is checked if the object the player selected is the right one or not. Depending on what the player chooses either the "End" phase will called directly or the "check" phase will be called.
  
  13. HandleCheckPhase.cs
 This script checks if the player is "GameOver", means he has no guesses left. If he has no guesses Left the script will set the game to the End Phase. Else it starts again with the Selection Phase.
  
  14. HandleEndPhase.cs
 The EndPhase script contains a method for restarting the game. It will clear the board accordingly and hand over to the Build Phase again.
  
  15. HandleUI.cs
  This script handles all the UI workflows. Depending on which phase is active it sets GameObjects either active or inactive. Also it contains of functions like OpenTutorialMenu(),BackToMenu() or OpenCustomMenu().
  
  16. SelectionManager.cs
  This script handles the functionality of selecting objects. When left clicking an object it will be selected and highlighted accordingly. This is done by using a Physics 2D raycast to get the position of the mouse and checks if it "hits" something.
  
  17. AudioManager.cs
  This script handles the audio of the game. It provides a methods to play a Sounds and Change Master Volume. Will be used by (24 VolumeSliderscript) and other scripts to Play sounds accordingly to a specific situation like Player has no lives left, Correct or wrong guess etc.
  
  18. GhostScript.cs
  This script is handling the behavior of the ghost enemies, which can spawn during SleepPhase. They also check for all existing SpawnPoints which are created during the Build phase and then picking a random one. After picking a random spawn point they move to this point until they have reached it. Then they pick another one again. It contains code for the animation of the ghost, moving (selecting spawnpoints and moving there) and also a OnGhostClick() method, to Destroy the ghost if the player clicks on it. After X amount of seconds the ghost is spawned the lighting will be reduced by 0.1 from 1 to have an overall effect on the board.
    
  19. BossEnemyScript.cs
  This script handles the behavior of the boss. To be fair, this would have been a perfect situation to inherit from the GhostScript.cs but I was under a little bit of time pressure. Therefore I have just reused the Move method here. For the Boss I have added life and a lifeBar for the UI, so it will not die instantly like the ghosts do. Also it will reduce the lighting of the scene by 0.5 from 1. 
  
  20. CameraFade.cs
  This script handles the "Close eye" illusion on the screen during sleep phase. Based on an animation curve the overall screen will fade for a few seconds. This brings the illusion of closing and opening the eye again.
  
  21. LightningScript.cs
  Very simple. Sets the global light to 1.0 at the beginning of starting the game.
  
  22. LightFollowMouse.cs
  I added a Point Light which is following the mouse, so a little area around the mouse is brighter than the rest. Also it acts as a kind of flashlight, if the Ghosts or the Boss darken the overall board.
  
  23. VolumeSliderscript.cs
  This script handles the Volume Slider for Master Volume.

  
 Thank you CS50 team for the great experience during the last weeks. It was a blast! Looking forward to do your CS50 Game development course too.
