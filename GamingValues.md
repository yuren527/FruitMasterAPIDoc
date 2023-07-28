# GamingValues
储存游戏策划有关数值的静态类
## internal struct FruitSpawnNumChanceRate
GamingValues类中嵌套的结构体，表示水果生成个数的几率表格行，包含五个几率值，分别为`Rate_1`, `Rate_2`, `Rate_3`, `Rate_4`, `Rate_5`, 其中`Rate_5`被省略。在构造函数中输入五个几率参数，即分别为生成一、二、三、四、五个水果的几率，五个几率值之和必须为100，其中五的几率值不需要储存，因为构造函数中已经限定了五的几率必然为100减去前四个几率之和。  

**Fields:**
|Access|Ty|Name|Description|
|---|---|---|---|
|private|int|Rate_1|生成一个水果的几率值，1-100|
|private|int|Rate_2|生成二个水果的几率值，1-100|
|private|int|Rate_3|生成三个水果的几率值，1-100|
|private|int|Rate_4|生成四个水果的几率值，1-100|

**Methods:**
|Access|RetTy|Name|Params|Description|
|---|---|---|---|---|
|public|void|FruitSpawnNumChanceRow|int rate_1<br/> int rate_2<br/> int rate_3<br/> int rate_4<br/> int rate_5|构造函数|
|public|int|GetRandomSpawnNum|void|根据几率值获得水果生成数量|  

***以上为FruitSpawnNumChanceRate类的方法和属性，下面开始列出GamingValues类的属性和方法。***    

**Fields:**  

|Access|Specifiers|Ty|Name|Description|
|---|---|---|---|---|
|private|static readonly|Dictionary<int, FruitSpawnNumChanceRow>|SpawnNumChanceRateTable|储存不同分段下生成不同数量水果的几率，键值表示该分段的上线，例如`{ 100, new FruitSpawnNumChanceRow(90, 10, 0, 0, 0) }`表示的是0-100分时，生成一个水果的几率为90%，生成两个水果的几率时10%，元素的排列必须按照键值大小由低到高|
|private|static readonly|Dictionary<int, int>|BombRateTable|不同分段下生成炸弹的几率，键的含义同上，值表示该分段下生成炸弹的几率，若生成炸弹将占用一个水果数量，例如`{200, 20}`表示101-200分段下，生成炸弹的几率为20%|
|private|const|float|BaseFruitLifetime|水果从生成到击中玩家的基础时间（秒）|
|private|static readonly|Dictionary<int, float>|FruitMoveSpeedMultiplierTable|不同分段下，水果移动的速度倍数|
|private|const|float|Max_BValueOfTrajectoryEquation|水果飞行抛物线使用`y=ax^2+b`进行描述，该属性为b值取值范围得最大值|
|private|const|float|Min_BValueOfTrajectoryEquation|水果飞行抛物线使用`y=ax^2+b`进行描述，该属性为b值取值范围得最小值|
|private|const|int|ComboPomegranate_Interval|允许生成连击石榴的分数间隔，例如，当该值为100时，分数到达500时可以允许生成总计5个连击石榴|
|private|static|int|ComboPomegranate_SpawnedCount|已生成的连击石榴数量|
|public|const|float|FreezeTime|冰冻香蕉触发效果的持续时间|
|public|const|float|MakeNewWaveCountdown|当所有水果都消失后，生成新一轮水果的时间间隔|
|public|const|float|SpawnFruitInterval|同一轮水果波次中，依次生成水果的时间间隔|
|public|const|float|ComboHitTime|连击石榴触发效果的持续时间|
|public|const|int|ComboHitLimit|连击石榴最大连击数|
|public|const|int|LifeCount|最大生命值|

**Methods:**  

|Access|specifiers|RetTy|Name|Params|Desc|
|---|---|---|---|---|---|
|private|static|T|GetTableValue|int score<br/>Dictionary<int, T>|从各个分段数值表格中获取对应值的模板函数|
|public|static|float|GetFruitLifetime|int wave 当前波数<br/>int score 当前分数|根据波数和分数，获得当前水果从生成到击中玩家的飞行时间，当波数小于等于5时，飞行时间为基础时间，大于5时，用分数获得飞行速度倍数后，除基础飞行时间，即为最终飞行时间|
|public|static|int|GetSpawnNum|int score 分数|根据分数获得新一波水果的生成个数，先获得该分段的生成数量表格行`FruitSpawnNumhanceRow`，然后从该行的几率中取得一个生成数量|
|public|static|bool|DoesSpawnBomb|int score|根据分数获得生成炸弹几率，然后判断是否要生成炸弹|
|public|static|bool|DoesSpawnComboPomegranate|int score|根据分数判断是否生成连击石榴,成功生成后已生成连击石榴数`ComboPomegranate_SpawnedCount	`将增加1|
|public|static|void|ResetPomegranateSpawnedCount|void|重置已生成连击石榴的数量为0，开始新游戏时调用|
|public|static|void|CalcTrajectoryEquationCoef|Vector3 origin 水果生成位置<br/>Vector3 target 水果飞行终点<br/>out float a<br/>out float b<br/> out float c|根据水果的起点和终点，获得其轨迹抛物线的abc系数值|
