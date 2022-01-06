---
title: Commonly Hooked Functions
weight: 3
author: dozens of helpful people in the community who post open source plugins
---

This is a list of functions that we use regularly, as well as when they fire. We'll never be able to document them all but hopefully this helps. Hook them with [Function Hooks](/functions/using_function_hooks/)

## [BallWrapper Caller](/bakkesmod_api/Classes/Wrappers/GameObject/BallWrapper/)
`"Function TAGame.Ball_TA.OnCarTouch"`  
This event is called when a ball hits a car. The caller is the ball that hit, and the first parameter is the [CarWrapper](/bakkesmod_api/Classes/Wrappers/GameObject/CarWrapper/) touched
```cpp
struct BallCarTouchParams {
    uintptr_t car;
}
```

`"Function TAGame.Ball_TA.OnHitGoal"`  
Happens when a goal is scored.  
Note that this event is also called during the goal replay, if the player does not cancel it. If you want to filter out the event during goal replay, you can hook to
`"Function GameEvent_Soccar_TA.ReplayPlayback.BeginState` and `"Function GameEvent_Soccar_TA.ReplayPlayback.EndState"` in order to e.g. set a `bool` flag while replay playback is active.

`"Function TAGame.Ball_TA.Explode"`  
Happens when a ball explodes. Usually this means a goal was scored

## [BoostWrapper Caller](/bakkesmod_api/Classes/Wrappers/GameObject/CarComponent/BoostWrapper/)
`"Function TAGame.VehiclePickup_Boost_TA.Pickup"`
Happens when a car picks up a boost
```cpp
struct BoostPickupParams {
    uintptr_t car;
}
```

## [CarWrapper Caller](/bakkesmod_api/Classes/Wrappers/GameObject/CarWrapper/)
`"Function TAGame.Car_TA.SetVehicleInput"`  
This event is called every physics tick while you are playing. This is 120 times per second. This can be highly useful but isn't perfect. It doesn't fire while spectating matches. While hosting a match it fires once per tick per car in the match

`"Function TAGame.Car_TA.OnHitBall"`  
This event is called when a car hits a ball. The caller is the [CarWrapper](/bakkesmod_api/Classes/Wrappers/GameObject/CarWrapper/) that hit, and the first parameter is the [BallWrapper](/bakkesmod_api/Classes/Wrappers/GameObject/BallWrapper/) touched
```cpp
struct CarBallTouchParams {
    uintptr_t ball;
}
```

## [ReplayDirectorWrapper Caller](/bakkesmod_api/Classes/Wrappers/GameEvent/ReplayDirectorWrapper/)
`"Function TAGame.ReplayDirector_TA.OnScoreDataChanged"`  
Happens when a goal replay shows a goal's scorer, assister, speed, and other information

## [ServerWrapper Caller](/bakkesmod_api/Classes/Wrappers/GameEvent/ServerWrapper/)
`"Function TAGame.GameEvent_Soccar_TA.EventMatchEnded"`  
Happens at the end of a match when the podium is shown. If someone quits early it is not called

`"Function TAGame.GameEvent_Soccar_TA.Destroyed"`  
Happens when the user quits an active game

`"Function TAGame.GameEvent_Soccar_TA.OnGameTimeUpdated"`  
Happens when the time on the game clock changes. Note that this is not real time. At 0:00 with the ball up it won't be called

`"Function TAGame.GameEvent_Soccar_TA.OnOvertimeUpdated"`  
Happens when overtime begins

`"Function GameEvent_Soccar_TA.ReplayPlayback.BeginState"`  
Happens when a goal replay begins

`"Function GameEvent_Soccar_TA.ReplayPlayback.EndState"`  
Happens when a goal replay ends

`"Function GameEvent_Soccar_TA.Active.StartRound"`  
Happens when all players are set for a kickoff. This happens at every single kickoff

`"Function GameEvent_Soccar_TA.Countdown.BeginState"`  
Happens at the start of a kickoff countdown or at the beginning/reset of freeplay

`"Function TAGame.GameEvent_Soccar_TA.Destroyed"`  
Happens when a match is left by the player. The caller may be null or unsafe to use as the match is destroyed

`"Function TAGame.GameEvent_TA.AddCar"`
Happens whenenever a car spawns. Useful if modifying car physics that may reset after a respawn

## [StatEventWrapper Caller](/bakkesmod_api/Classes/Wrappers/GameObject/Stats/StatEventWrapper/)
### [StatEventWrapper functions are explained here](/functions/stat_events/)
`"Function TAGame.GFxHUD_TA.HandleStatTickerMessage"`  
Called any time a stat should appear on the top right ticker, even if it's disabled  
`"Function TAGame.GFxHUD_TA.HandleStatEvent"`  
Called any time a stat should appear in the center of the screen, even if it's disabled  

## [TeamWrapper Caller](/bakkesmod_api/Classes/Wrappers/GameObject/TeamWrapper/)
`"Function TAGame.Team_TA.PostBeginPlay"`  
Happens twice at the start of a match, once per team that is totally joined and ready. It can be used to detect the start of a match. This only works when the player is in the match as it starts, so it's inconsistent in casual where you can join matches in progress

## [TrainingEditor Caller](bakkesmod_api/Classes/Wrappers/GameEvent/TrainingEditorWrapper/)
`"Function TAGame.TrainingEditorMetrics_TA.TrainingShotAttempt"`  
Called whenever an attempt is started in a custom training pack. An attempt counts as started if the player starts moving in any way. Selecting a different shot, or resetting the shot does not fire this event (until the player moves).
If you are only interested in the initial ball touch for each shot, you can react to only the first `"Function TAGame.Ball_TA.OnCarTouch"` event after each `TrainingShotAttempt` for example.

`"Function TAGame.GameEvent_TrainingEditor_TA.OnInit"`  
Called whenever a new training pack is being loaded, or the current training pack was restarted.  
Be aware that `_gameWrapper->IsInCustomTraining()` will not yet return `true` at this point.

## Unwrapped callers
`"Function TAGame.GFxData_MainMenu_TA.MainMenuAdded"`  
Happens when the main menu is opened, either through booting the game or leaving a mode

`"Function Engine.GameViewportClient.Tick"`  
This event is called every rendered frame. This is very powerful, but again can have issues. Different framerates and stutters can make the number of ticks different on different PCs
