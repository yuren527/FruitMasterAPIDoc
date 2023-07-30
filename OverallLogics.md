# OverallLogics : MonoBehaviour
最顶层的逻辑操作，主要用于控制游戏正式开始之前，或游戏结束后的一些界面和效果操作，游戏进行时的逻辑不在其中。

## OverallLogics中的引用
|Access|Ty|Name|Desc|
|---|---|---|---|
|private|Button|_startGameBtn|开始游戏按钮|
|private|GameObject|GameOverPanel|游戏结束界面|
|private|GameObject|Stains|游戏结束时出现的污渍|
|private|TextMeshProUGUI|Scores|游戏结束时显示的最终分数|
|private|Image|Logo|游戏Logo|
|private|Image|MenuBackground|游戏开始界面背景|
|private|Button|ShutdownBtn|退出游戏按钮|
|private|Button|ResetAimBtn|重置准星按钮|

## OverallLogics中的属性
该类中的属性全部都是用于控制游戏结束动画效果，可以略过
|Access|Specifiers|Ty|Name|Desc|
|---|---|---|---|---|
|private| |bool|bPlayingGameOverAnim|是否在播放游戏结束动画|
|private| |float|TimeGameOverAnimPlayed|游戏结束动画播放的时长|
|private|readonly|float[]|StainEmergeTimes|游戏结束污渍出现的时间|
|private|const|float|GameOverPanelEmergeTime|游戏结束字样出现的时间|
|private|const|float|GameOverFinishAnimTime|游戏结束动画完成的时间|

## OverallLogics中的方法
|Access|Specifiers|RetTy|Name|Params|Desc|
|---|---|---|---|---|---|
|public| |void|StartGame|void|开始游戏，调用GamingManager中的开始游戏，并设置本对象中关联的界面元素|
|private| |void|OnGameEnd|EndGameReason reason<br/>int scores|游戏结束时触发的事件函数，绑定在GamingManager上，主要内容是正确显示界面|
|private| |void|OnSmartBowConnectionLost|void|智能弓箭连接丢失时触发的函数，目前只暂停游戏|
|private| |void|Shutdown|void|**关闭游戏，该函数需要在游戏整合时实现，目前留空**|
|private| |void|ResetAim|void|重置准心按钮函数|  

## 其他
**本来中的Start和Update函数中，只做了一些指针的赋值和界面设置的操作，在此不再赘述，如有需要可阅读源代码**
