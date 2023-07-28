# GamingManager ：MonoBehaviour #
游戏进行时的主要游戏逻辑控制类

## public enum EndGameReason
游戏结束原因，`UserExit`，`GameOver`，分别代表玩家主动退出和游戏结束。

## GamingManager的引用
|Access|Ty|Name|Desc|
|---|---|---|---|
|private|SmartBowManager|bowManager|智能弓接口类的引用，在游戏开发过程中用于连接和调试智能弓，整合阶段已弃用|
|private|AudioManager|audioManager|用于管理音乐和音效的管理组件引用|
|private|AudioSource|BowFireAudio|发射弓箭的音效|
|private|AudioSource|LooseLifeAudio|失去生命值的音效|
|private|GameObject|FruitPrefab|水果预制体|
|private|GameObject|ArrowPrefab|箭预制体|
|private|GameObject|ComboTextPrefab|连击浮动字体预制体|
|private|GameObject|CriticalTextPrefab|暴击浮动字体预制体|
|private|GameObject|AddLifeTextPrefab|增加生命值浮动字体预制体|
|private|GameObject|GamingUIContainer|游戏界面父物体|
|private|Image|AimingCross_Img|准心图标|
|private|GameObject|Resume_btn|继续按钮|
|private|GameObject|Pause_btn|暂停按钮|
|private|GameObject|Exit_btn|退出按钮|
|private|GameObject|Life_0_img|失去生命值图标|
|private|GameObject|Life_1_img|失去生命值图标|
|private|GameObject|Life_2_img|失去生命值图标|
|private|TextMeshProUGUI|ScoreText|分数显示|
|private|CannonController|Cannon|大炮的引用|
|private|GameObject|FullscreenClearBubble|全屏消灭泡泡预制体|
|private|int|StartScores|开始分数，用于从高分值开始的调试|
|private|float|AimingCrossPosMultiplier|准心位置调节值，用于调节准心关联智能弓姿态后的灵敏度|
|private|GameObject|IceCube|冰冻香蕉全屏效果的场景物体引用|
|private|Camera|Cam|主摄像机的引用|

## GamingManager的属性
|Access|Specifiers|Ty|Name|Desc|
|---|---|---|---|---|
|public|const|float|ArrowSpeedMultiplier|弓箭飞行速度增量系数|
|public|const|float|GravityMultiplier|重力增量系数|
|private| |Quaternion|CurrentRotation|当前智能弓姿态值，从智能弓接口中读取|
|private| |int|Scores|当前分数|
|private| |int|Combos|当前连击数|
|private| |int|Life|当前生命值|
|private| |bool|bGameStarted|游戏是否开始|
|private| |bool|bGamePaused|游戏是否暂停中|
|private| |bool|bFreeze|冰冻香蕉效果开关|
|private| |bool|bFreezing|是否处于冰冻香蕉效果中|
|private| |float|FreezeTimeEclipsed|已处于冰冻香蕉效果中的时常|
|private| |bool|bEnterComboTime|连击石榴效果开关|
|private| |bool|bComboTime|是否处于连击石榴效果中|
|private| |float|ComboTimeEclipsed|已处于连击石榴效果中的时长|
|private| |FruitBehavior|ActiveComboPomegranate|当前被激活的连击石榴引用|
|private| |List\<Vector3\>|FruitVelocityCache_Linear|当前场景中所有水果的物理线速度缓存，用于在退出连击石榴效果时重新赋予各个水果使其继续运动，目前版本中水果运动是Kinematic，因此该属性并没有实际效果，可能在后续删除|
|private| |List\<Vector3\>|FruitVelocityCache_Angular|当前场景中所有水果的物理角速度缓存，用于在退出连击石榴效果时重新赋予各个水果使其继续运动，目前版本中水果运动是Kinematic，因此该属性并没有实际效果，可能在后续删除|
|private| |float|MakeNewWaveCountdown|生成新一轮水果波次的倒计时，只有当场景中没有水果时才会开始，并在生成后重置|
|private| |List\<GameObject\>|FruitInField|当前场景中存在的水果|
|private| |List\<FruitType\>|FruitsPendingSpawn|当前波次中预备生成的水果列表，元素用水果类型表示，生成时按照对应类型生成新的水果物体|
|private| |float|TimeSinceLastFruitSpawn|上一个水果生成后的计时，用于控制同一波次中水果的生成间隔|
|public|event|GameEndDelegate|OnGameEnd|游戏结束事件|
|private| |int|FruitWaves|当前水果波次|
|private| |Vector2|ScreenMidPoint|屏幕中心点像素值，用于智能弓姿态和准心位置的换算|
|private| |float|FireColldownTime_Current|射击冷却倒计时，射击冷却用于防止一次射击瞬间触发两次射击事件|
|private|const|float|FireCooldownTime|射击冷却秒数|

## GamingManager的方法
|Access|Specifiers|RetTy|Name|Params|Desc|
|---|---|---|---|---|---|
|public| |void|StartGame|void|开始游戏，该方法主要进行分数、连击数、生命值等数值的重置，并激活游戏进行时相关UI，绑定智能弓姿态接口，并将生成水果的方法绑定到大炮开火的事件上|
|public| |void|PauseGame|void|暂停游戏，隐藏准星和暂停按钮，显示继续按钮，将Time.timeScale设为0|
|public| |void|ResumeGame|void|继续游戏，暂停游戏的反向操作|
|public| |void|IsGameStarted|void|游戏是否已开始|
|public| |void|IsGamePaused|void|游戏是否已暂停|
|public| |void|MakeFruitsWave|void|生成一波新的预生成水果，此阶段按照分数和波次生成一个新的水果队列，此阶段没有实际的物体产生，新队列生成后水果波次+1|
|public| |void|SpawnAFruit|void|从水果生成队列中生成一个实际的水果，赋予水果对应的运动属性参数，并将水果的被击中和击中玩家事件绑定到对应的函数上|
|private| |void|AFruitArrived|FruitBehavior fb|水果击中玩家时调用的事件函数，计算击中后的生命值，生成击中特效，消灭水果，生命值耗尽时结束游戏|
|private| |void|EndGame|EndGameReason reason|结束游戏，重置大炮位置，解绑大炮发射事件和智能弓姿态事件等一一系列操作|
|private| |void|DelayEndGame|EndGameReason reason 结束原因<br/>float wat_secs 延迟秒数|延迟结束游戏|
|private|static|void|DelayDestroy|MonoBehaviour excutor 消灭操作的执行者<br/> GameObject to_destroy 拟消灭物体<br/>float wait_secs 延迟事件|延迟消灭物体|
|public| |void|ExitGame|void|退出游戏，主动退出|


