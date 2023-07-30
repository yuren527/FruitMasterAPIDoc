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
|public| |bool|IsGameStarted|void|游戏是否已开始|
|public| |bool|IsGamePaused|void|游戏是否已暂停|
|public| |void|MakeFruitsWave|void|生成一波新的预生成水果，此阶段按照分数和波次生成一个新的水果队列，此阶段没有实际的物体产生，新队列生成后水果波次+1|
|public| |void|SpawnAFruit|void|从水果生成队列中生成一个实际的水果，赋予水果对应的运动属性参数，并将水果的被击中和击中玩家事件绑定到对应的函数上|
|private| |void|AFruitArrived|FruitBehavior fb|水果击中玩家时调用的事件函数，计算击中后的生命值，生成击中特效，消灭水果，生命值耗尽时结束游戏|
|private| |void|EndGame|EndGameReason reason|结束游戏，重置大炮位置，解绑大炮发射事件和智能弓姿态事件等一一系列操作|
|private| |void|DelayEndGame|EndGameReason reason 结束原因<br/>float wat_secs 延迟秒数|延迟结束游戏|
|private|static|void|DelayDestroy|MonoBehaviour excutor 消灭操作的执行者<br/> GameObject to_destroy 拟消灭物体<br/>float wait_secs 延迟秒数|延迟消灭物体|
|public| |void|ExitGame|void|退出游戏，主动退出|
|private| |void|FireArrow|float speed<br/> Vector dir|发射弓箭的内部方法|
|private| |void|SmartBowFireArrow|float speed|使用智能弓发射箭的方法，此方法绑定在智能弓接口的发射事件上，由事件调用|
|private| |void|UpdateRotation|Quaternion q|更新智能弓姿态的函数，由智能弓接口的事件调用|
|private| |void|MouseFireArrow|Vector3 screenPos|用鼠标点击或手机上触屏点击来发射弓箭的函数，在Update中监听输入并调用|
|private| |void|OnFruitHit|FruitBehavior fb 命中的水果<br/>Vector3 hitPoint 命中位置<br/>bool bCritical 是否为暴击|水果被击中后调用的方法，水果生成后该方法会被绑定到水果的OnHitEvent事件|
|private| |bool|GainScore|int score 将获得的分数|获得分数，如果达到回复生命值的条件，则返回true，否则返回false|
|private| |bool|UpdateLife|bool bDecrease|更新生命值，当参数为true时扣除一点生命值，否则回复一点生命值，当生命值降为0时返回true|
|private| |void|MissShoot|void|当没有击中水果时调用的函数，主要用于重置连击数|
|private| |void|ShowGamingUI|bool b|隐藏或显示游戏UI|
|public| |void|DestroyFruit|FruitBehavior fb 要消灭的水果|消灭水果，除了正常抹去物体外，还把水果从列表中移除，同时调用水果被击碎的效果|
|public| |void|SpawnStain|Sprite stain<br/>Vector3 position3d_ws|水果击中时生成屏幕污渍效果的方法|

## GamingManager中Awake函数中的操作
- 获取引用
  ```
  audioManager = gameObject.GetComponent<AudioManager>();
        Debug.Assert(audioManager);
        bowManager = gameObject.GetComponent<SmartBowManager>();
        Debug.Assert(bowManager);
        Debug.Assert(Cannon);
  ```
- 获取屏幕像素并为屏幕中心点像素赋值
  ```
  ScreenMidPoint = new Vector2(Screen.width / 2, Screen.height / 2);
  ```

## GamingManager中Start函数中的操作
- 为按键绑定事件
  ```
  Resume_btn.GetComponentInChildren<Button>().onClick.AddListener(ResumeGame);
  Pause_btn.GetComponentInChildren<Button>().onClick.AddListener(PauseGame);
  Exit_btn.GetComponentInChildren<Button>().onClick.AddListener(ExitGame);
  ```
- 绑定智能弓发射事件
  ```
  SmartBowHelper.GetInstance().OnShooting += SmartBowFireArrow;
  ```
- 将各类UI调整到初始状态
  ```
  AimingCross_img.gameObject.SetActive(false);
  Resume_btn.gameObject.SetActive(false);
  ShowGamingUI(false);
  ```
- 设置物理重力大小
  ```
  Physics.gravity = new Vector3(0.0f, -9.8f * GravityMultiplier, 0.0f);
  ```
- 音效延迟调整
  ```
  BowFireAudio.timeSamples = 3000;
  ```
- 播放背景音乐
  ```
  AudioSource backgroundMusic = gameObject.GetComponent<AudioSource>();
  backgroundMusic.clip = audioManager.GetBackgroundMusic();
  backgroundMusic.Play();
  ```

## GamingManager中Update函数中的操作
- 发射弓箭冷却
  ```
  if (FireCooldownTime_Current > 0f)
        {
            FireCooldownTime_Current = Mathf.Clamp(FireCooldownTime_Current - Time.deltaTime, 0f, FireCooldownTime);
        }
  ```
- 监听鼠标或触屏点击发射
  ```
  if (Input.GetMouseButtonDown(0))
            {
                MouseFireArrow(Input.mousePosition);
            }
  ```
- 生成水果或生成新的水果波次
  ```
  if (FruitsPendingSpawn.Count > 0)
            {
                if (TimeSinceLastFruitSpawn > GamingValues.SpawnFruitInterval)
                {
                    if (!Cannon.IsFireAnimating())
                    {
                        Cannon.TriggerShot();
                        // Skip reseting fire interval when this is the last pending fruit
                        // since we need it to fire immediately when firing the first fruit of a new wave
                        if (FruitsPendingSpawn.Count == 1)
                        {
                            TimeSinceLastFruitSpawn = 0f; 
                        }
                    }
                    else
                    {
                        // do nothing ,wait for cannon fire animation to finish
                    }
                }
                else
                {
                    TimeSinceLastFruitSpawn += Time.deltaTime;
                }
            }
            else // No pending spawning fruit
            {
                if (FruitsInField.Count == 0)
                {
                    if (MakeNewWaveCountdown > 0)
                    {
                        MakeNewWaveCountdown -= Time.deltaTime;
                    }
                    else
                    {
                        MakeFruitsWave();
                        MakeNewWaveCountdown = GamingValues.MakeNewWaveCountdown;
                    }
                }
            }
  ```
- 更新准心位置
  ```
  Vector3 raw_screen_pos = Cam.WorldToScreenPoint(Cam.transform.position + (CurrentRotation * Vector3.forward));
                float dist_to_mid_x = (raw_screen_pos.x - ScreenMidPoint.x) * AimingCrossPosMultiplier;
                float dist_to_mid_y = (raw_screen_pos.y - ScreenMidPoint.y) * AimingCrossPosMultiplier;

                Vector3 scrren_pos = new Vector3(Mathf.Clamp((dist_to_mid_x + ScreenMidPoint.x), 0f, 2 * ScreenMidPoint.x), Mathf.Clamp((dist_to_mid_y + ScreenMidPoint.y), 0f, 2 * ScreenMidPoint.y), 1f);

                AimingCross_img.gameObject.GetComponent<RectTransform>().position = scrren_pos;
  ```
- 冰冻香蕉和连击石榴效果开关和执行
  ```
  // Freeze functionality
            if (bFreeze)
            {
                Time.timeScale = 0.5f;
                bFreezing = true;
                bFreeze = false;
                foreach (var fruit in FruitsInField)
                {
                    fruit.GetComponent<FruitBehavior>().Frost();
                }
            }
            if (bFreezing)
            {
                // Freeze visual effect
                float freeze_alpha;
                if (FreezeTimeEclipsed < 0.1f)
                {
                    freeze_alpha = Mathf.Lerp(1f, 0.5f, FreezeTimeEclipsed / 0.1f);
                }
                else
                {
                    freeze_alpha = Mathf.Lerp(0.5f, 1f, (FreezeTimeEclipsed - 0.1f) / (GamingValues.FreezeTime - 0.1f));
                }
                MeshRenderer renderer = IceCube.GetComponent<MeshRenderer>();
                renderer.material.SetFloat("_Cutoff", freeze_alpha);

                FreezeTimeEclipsed += (Time.deltaTime / Time.timeScale); // deltaTime is scaled by timeScale, divide it by timeScale will fix it
                if (FreezeTimeEclipsed >= GamingValues.FreezeTime)
                {
                    Time.timeScale = 1.0f;
                    FreezeTimeEclipsed = 0f;
                    bFreezing = false;
                    foreach (var fruit in FruitsInField)
                    {
                        fruit.GetComponent<FruitBehavior>().UnFrost();
                    }
                }              
            }

            // ComboTime functionality
            if (bEnterComboTime)
            {
                bComboTime = true;
                bEnterComboTime = false;
                ComboTimeEclipsed = 0f;
            }
            if (bComboTime)
            {
                ComboTimeEclipsed += Time.deltaTime;
                if (ComboTimeEclipsed > GamingValues.ComboHitTime)
                {
                    bComboTime = false;
                    ComboTimeEclipsed = 0f;

                    foreach (var f in FruitsInField)
                    {
                        FruitBehavior fr = f.GetComponent<FruitBehavior>();
                        fr.bMoveAlongTrajectory = true;
                    }
                    FruitVelocityCache_Linear.Clear();
                    FruitVelocityCache_Angular.Clear();

                    DestroyFruit(ActiveComboPomegarante);
                    ActiveComboPomegarante = null;
                }
                else
                {
                    foreach (var f in FruitsInField)
                    {
                        FruitBehavior fr = f.GetComponent<FruitBehavior>();
                        fr.bMoveAlongTrajectory = false;
                    }
                }
            }
  ```
