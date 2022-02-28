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
 This script is the main script to build individual game boards, based on level or custom data the player is handing over via UI.
  It places Inner and Outter Tiles of the board and also creates the SpawnPoints, where later objects can spawn on the board. How big the board will be will be defined in a independent "Scriptable Object", which is in this case just a data container to store values. The values change depending on which level the player wants to play.
  
  
  
  
  2. Spawner.cs
  3. CreateSpawnPoints.cs
  4. AudioManager.cs
  BossEnemyScript.cs
  CameraFade.cs
  HandleCheckPhase.cs
  HandleEndPhase.cs
  HandleLookPhase.cs
  HandleMainMenuPhase.cs
  HandleSelectionPhase.cs
  HandleSleepPhase.cs
  HandleUI.cs
  LightFollowMouse.cs
  LightingScript.cs
  ObjectID.cs
  SelectionManager.cs
  SpawnLocationManager.cs
  TimerBehavior.cs
  VolumeSliderscript.cs
  

  
 Thank you CS50 team for the great experience during the last weeks. It was a blast! Looking forward to do your CS50 Game development course too.
