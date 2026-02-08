---
sidebar_position: 1
---

# 介四简介

只是朴实无华的Pvz2_Gardendless版本自定义关卡教程

本教程适用于想要自己创作关卡却无从下手的人

也对于将要高三离开的自己

# Before Started

在开始创作之前的预备知识

## 写关工具

*文本编辑器*:Json可视化强的工具，首推VScode和notepad++

## 预备知识

*Json格式*:关卡文件格式为Json，请熟悉其语法

*Json5格式*:关卡文件也可以用Json5，但本教程用前者

# 认识关卡结构

这是一个基本的外包装，其中*"objects": []*列表为关卡内容

```json
{
	"#comment": "Bird's crafting",
	"objects": [{},{},{},{},{},{}],
	"version": 1
	
```

只需要在*"objects": []*列表中填入对应的对象就可以构成一个关卡

先填入*"LevelDefinition"*，直译即为关卡定义

*"objclass"*可以看作一个函数

*"objdata"*可以看作自变量

*"Description"*为关卡描述，显示在刚进入关卡的下方

*"LevelNumber"*为系列关卡序号

*"Loot"*不明

*"Modules"*相当于第-1波，可以填其他的函数，"RTID()"相当于函数名，

	其中：

	*"RTID(StandardIntro@LevelModules)"*基本必填

	*"RTID(SeedBank@CurrentLevel)"*为选卡界面

	*"RTID(NewWaves@CurrentLevel)"*为动态出怪

*"Name"*为关卡名称，显示在旗帜进度条附近

*"NameMultiLanguage"*为语言本地化键

*"WritenBy"*为作者，但是暂时无用

*"NormalPresentTable"**"ShinyPresentTable"*为关卡奖励，照着这个填就可以了

*"StartingSun"*为初始阳光

*"Sandbox"*为是否为沙盒模式

*"StageModule"*为地图，例如冰河，摇滚等

```json
{
	"#comment": "Bird's crafting",
	"objects": [{
			"objclass": "LevelDefinition",
			"objdata": {
				"Description": "遗忘之前：<PLAYERNAME>",
				"LevelNumber": 5,
				"Loot": "RTID(DefaultLoot@LevelModules)",
				"Modules": [
					"RTID(StandardIntro@LevelModules)",
					"RTID(SeedBank@CurrentLevel)",
					"RTID(NewWaves@CurrentLevel)"
				],
				"Name": "七影蝶：记忆-<LEVELNUMBER>",
				"NameMultiLanguage": {
					"en": "七影蝶：ForNever",
					"zh": "七影蝶：ForNever"
				},
				"WritenBy": "Birdscraft",
				"NormalPresentTable": "egypt_normal_01",
				"ShinyPresentTable": "egypt_shiny_01",
				"StartingSun":225,
				"Sandbox":false,
				"StageModule": "RTID(SummerNightsStage@LevelModules)"
			}
		}],
	"version": 1
	
```

接下来添加*"WaveManagerProps"* ，*"aliases"*相当于函数名

其中：

	*"SuppressFlagZombie"*为是否删除旗帜僵尸

	*"FlagWaveInterval"*，*"WaveCount"*共同决定旗帜数（非波数）

	*"Waves"*为波，其中有几个*[]*就有几波，其中：

		*[]*空对象即为空波
		
这里定义函数*"Wave0"*举例（当然正式写关都要定义）

	*"aliases"*为函数名，可任意填写

	*"AdditionalPlantfood"*为本波叶绿素数量

	*"Zombies"*中列举了本波出场的僵尸

在 *"NewWaves"* 中调用 *"WaveManagerProps"*

 *"DynamicZombies"*为动态出怪，较为复杂，详见下文

 七个空{}对应七个难度GFEDCBA，一定要填满，不能只填四五个

```json
{
	"#comment": "Bird's crafting",
	"objects": [{
			"aliases": ["WaveManagerProps"],
			"objclass": "WaveManagerProperties",
			"objdata": {
				"SuppressFlagZombie":true,
				"FlagWaveInterval": 100,
				"WaveCount": 1,
				"Waves": [
                    [],
					["RTID(Wave0@CurrentLevel)"]
                    []
				]
			}
		},{
			"aliases": [
				"NewWaves"
			],
			"objclass": "WaveManagerModuleProperties",
			"objdata": {
				"DynamicZombies": [
				{},{},{},{},{},{},{}
				],
				"WaveManagerProps": "RTID(WaveManagerProps@CurrentLevel)"
			}
		},{
			"aliases": [
				"Wave0"
			],
			"objclass": "SpawnZombiesJitteredWaveActionProps",
			"objdata": {
				"AdditionalPlantfood": 0,
				"Zombies": [
					{
						"Type": "RTID(abbot_torch@ZombieTypes)"
					},
					{
						"Type": "RTID(summer_armor1@ZombieTypes)"
					},
					{
						"Type": "RTID(summer_armor2@ZombieTypes)"
					}
					
				]
			}
		},{
			"objclass": "LevelDefinition",
			"objdata": {
				"Description": "遗忘之前：<PLAYERNAME>",
				"LevelNumber": 5,
				"Loot": "RTID(DefaultLoot@LevelModules)",
				"Modules": [
					"RTID(StandardIntro@LevelModules)",
					"RTID(SeedBank@CurrentLevel)",
					"RTID(NewWaves@CurrentLevel)"
				],
				"Name": "七影蝶：记忆-<LEVELNUMBER>",
				"NameMultiLanguage": {
					"en": "七影蝶：ForNever",
					"zh": "七影蝶：ForNever"
				},
				"WritenBy": "Birdscraft",
				"NormalPresentTable": "egypt_normal_01",
				"ShinyPresentTable": "egypt_shiny_01",
				"StartingSun":225,
				"Sandbox":false,
				"StageModule": "RTID(SummerNightsStage@LevelModules)"
			}
		}],
	"version": 1
}

```

恭喜你，你的第一个关卡已经制作成功！

下面列举函数，用了.js的格式方便注释，一定要记得改json

虽然不全，但是也够用了

```js
{
	//放在["WaveManagerProps"]的"Waves": [[],[]]中或"LevelDefinition"的"Modules":[]中
    
    //选卡

	"RTID(SeedBank@CurrentLevel)",//在"LevelDefinition"的"Modules":[]中调用

	{
			"aliases": [
				"SeedBank"
			],
			"objclass": "SeedBankProperties",
			"objdata": {
				"PresetPlantList": [
				{
					"Level": -1,
					"PlantType": "sunflower"
				},{
					"Level": -1,
					"PlantType": "citron"
				},{
					"Level": -1,
					"PlantType": "chilibean"
				},{
					"Level": -1,
					"PlantType": "stallia"
				},{
					"Level": -1,
					"PlantType": "sunbean"
				},{
					"Level": -1,
					"PlantType": "marigold_blue"
				},{
					"Level": -1,
					"PlantType": "sapfling"
				}
				],
				"SelectionMethod": "preset"//preset:锁卡 chooser:自选卡
			}
		},
		
		//关卡流程

		{
			"aliases": ["WaveManagerProps"],
			"objclass": "WaveManagerProperties",
			"objdata": {
				"FlagWaveInterval": 100,
				"WaveCount": 1,
				"LevelJamList": [
					"jam_rap"
				],//魔音音乐
				"SuppressFlagZombie":true,//旗帜
				"Waves": [
					["RTID(Wave1@CurrentLevel)","RTID(Wave1GraveSpawn@CurrentLevel)"],
					["RTID(Wave2@CurrentLevel)","RTID(Wave2StormSpawn@CurrentLevel)"],
					["RTID(Wave3@CurrentLevel)"],
					["RTID(Wave4@CurrentLevel)","RTID(Wave4StormSpawn@CurrentLevel)"],
					["RTID(Wave5@CurrentLevel)","RTID(Wave5StormSpawn@CurrentLevel)"],
					["RTID(Wave6@CurrentLevel)"],
					["RTID(Wave7@CurrentLevel)","RTID(Wave7StormSpawn@CurrentLevel)"],
					["RTID(Wave8@CurrentLevel)"],
					["RTID(Wave9@CurrentLevel)","RTID(Wave9StormSpawn@CurrentLevel)"],
					["RTID(Wave10@CurrentLevel)"]
				]
			}
		},
		//天降阳光
		"RTID(DefaultSunDropper@LevelModules)"
		//出怪和动态难度
		"RTID(NewWaves@CurrentLevel)",//在"LevelDefinition"的"Modules":[]中调用

		{
			"aliases": [
				"NewWaves"
			],
			"objclass": "WaveManagerModuleProperties",
			"objdata": {
				"DynamicZombies": [
				{},
				{},
				{},
				{},
				{
					"PointIncrementPerWave": 32,//点数每波增量
					"StartingPoints": 125,//起始点数（可为负数）
					"StartingWave": 1,//从第几波开始应用
					"ZombiePool": [
						"RTID(mummy_armor1@ZombieTypes)",
						"RTID(mummy_armor2@ZombieTypes)",
						"RTID(mummy_armor4@ZombieTypes)"
					]
				},
				{
					"PointIncrementPerWave": 84,
					"StartingPoints": 500,
					"StartingWave": 4,
					"ZombiePool": [
						"RTID(mummy_armor1@ZombieTypes)",
						"RTID(mummy_armor2@ZombieTypes)",
						"RTID(lostcity_bug_armor1@ZombieTypes)",
						"RTID(lostcity_bug_armor2@ZombieTypes)",
						"RTID(camel_manyhump@ZombieTypes)",
						"RTID(west_bull@ZombieTypes)",
						"RTID(pharaoh@ZombieTypes)",
						"RTID(mummy_armor4@ZombieTypes)"
					]
				},
				{
					"PointIncrementPerWave": 45,
					"StartingPoints": 400,
					"StartingWave": 7,
					"ZombiePool": [
						"RTID(tomb_raiser@ZombieTypes)",
						"RTID(tomb_raiser@ZombieTypes)",
						"RTID(tomb_raiser@ZombieTypes)",
						"RTID(tomb_raiser@ZombieTypes)",
						"RTID(explorer@ZombieTypes)",
						"RTID(mummy_armor4@ZombieTypes)",
						"RTID(mummy_armor4@ZombieTypes)",
						"RTID(mummy_flag_veteran@ZombieTypes)",
						"RTID(mummy_flag_veteran@ZombieTypes)",
						"RTID(future_jetpack_veteran@ZombieTypes)",
						"RTID(future_jetpack_veteran@ZombieTypes)"
					]
				}
				],
				"WaveManagerProps": "RTID(WaveManagerProps@CurrentLevel)"
			}
		},
		
		//关卡定义
		{
			"objclass": "LevelDefinition",
			"objdata": {
				"Description": "TEST",
				"LevelNumber": 1,//系列关卡
				"SandBoxMode":true,//沙盒模式
				"Loot": "RTID(DefaultLoot@LevelModules)",
				"Modules": [
					"RTID(StandardIntro@LevelModules)",
					"RTID(SeedBank@CurrentLevel)",
					"RTID(ZombiesDeadWinCon@LevelModules)",
					"RTID(DefaultZombieWinCondition@LevelModules)",
					"RTID(NewWaves@CurrentLevel)",
					"RTID(Gravestones@CurrentLevel)"
				],
				"Name": "TEST ",
				"NameMultiLanguage": {
					"en": "TEST",
					"zh": "TESDF"
				},
				"WritenBy": "Birdscraft",
				"NormalPresentTable": "egypt_normal_01",
				"ShinyPresentTable": "egypt_shiny_01",
				"StartingSun":2250,//初始阳光
				"StageModule": "RTID(FutureStage@LevelModules)"//地图
			}
		},
		
		//关卡必有结构
        //在"LevelDefinition"的"Modules":[]中调用
		"RTID(StandardIntro@LevelModules)",
		"RTID(ZombiesDeadWinCon@LevelModules)",
		"RTID(DefaultZombieWinCondition@LevelModules)",
		"RTID(NewWaves@CurrentLevel)",
		
		//波次定义
		{
			"aliases": [
				"Wave1"
			],
			"objclass": "SpawnZombiesJitteredWaveActionProps",
			"objdata": {
				"AdditionalPlantfood": 0,
				"Zombies": [
					{
						"Style": "snowstorm",                  //暴风雪
						"Column":"1",                          //第一列
						"Type": "RTID(future_imp@ZombieTypes)",//僵尸名
						"Row": 4                               //第四行
					},
					{
						"Style": "sandstorm",                  //沙尘暴
						"Column":"1",
						"Type": "RTID(future_imp@ZombieTypes)",
						"Row": 4
					},
					{//地上冒出僵尸
						"Column":"1",
						"Type": "RTID(future_imp@ZombieTypes)",
						"Row": 4
					},
					{
						"Row":1,
						"Type": "RTID(future_armor4@ZombieTypes)"
					}
				]
			}
		},
		
		//计时器
		{
			"aliases": [
				"Scheduler0"
			],
			"objclass": "WaveSchedulerProps",
			"objdata": {
				"TimeBeforeFirst": {//第一次前时间
				"Max": 1.0,
				"Min": 1.0
				},
				"TimeBetween": {//重复间隔
				"Max": 0,
				"Min": 0
				},
				"Repeat": {//重复次数
				"Max": 1,
				"Min": 1
				},
				"RewardWhenEnded": false,
				"SuppressRewardState": true,
				"Events": [
					"RTID(Wave1StormSpawn@CurrentLevel)",
					"RTID(Wave1StormSpawn@CurrentLevel)"
				]
			}
		},
		//随机事件容器
		{
			"aliases": [
				"rweasels"
			],
			"objclass": "RandomEventsProps",
			"objdata": {
				"EventCount": {//抽取次数
				"Min": 1,
				"Max": 1
				},
				"Events": [
					"RTID(weaselp0@CurrentLevel)",
					"RTID(weaselp1@CurrentLevel)",
					"RTID(weaselp2@CurrentLevel)",
					"RTID(weaselp3@CurrentLevel)",
					"RTID(weaselp4@CurrentLevel)"
				]
			}
		},
		
		//场上原有的植物or固定位置药水

		{
			"aliases": [
				"FrozenPlantPlacement"
			],
			"objclass": "InitialPlantProperties",
			"objdata":{
				"InitialPlantPlacements": [
				{
					"GridX": 8,
					"GridY": 0,
					"TypeName": "shadowshroom",
					"Level": -1
				},
				{
					"GridX": 3,
					"GridY": 0,
					"TypeName": "zombiepotion_invisibility",//隐身药水
					"Level": -1
				},
				{
					"GridX": 8,
					"GridY": 2,
					"TypeName": "shadowshroom",
					"Level": -1
				},
				{
					"GridX": 8,
					"GridY": 4,
					"TypeName": "shadowshroom",
					"Level": -1
				},
				{
					"GridX": 8,
					"GridY": 1,
					"TypeName": "moonflower",
					"Level": -1
				},
				{
					"GridX": 8,
					"GridY": 3,
					"TypeName": "moonflower",
					"Level": -1
				}
				]
			}
		},
		//场上原有的僵尸
		{
			"aliases":[
				"FrozenZombiePlacement"
			],
			"objclass":"InitialZombieProperties",
			"objdata":{
				"InitialZombiePlacements":[
				{
				"Condition":"icecubed",//冰块
				"GridX":4,
				"GridY":2,
				"TypeName":"pharaoh"
				}
				]
			}
		},
		//挑战模式
		{
			"aliases":[
				"ChallengeModule"
			],
			"objclass":"StarChallengeModuleProperties",
			"objdata":{
				"Challenges":[
					["RTID(DoNotPlantBeforeLine@CurrentLevel)"]
				],
				"ChallengesAlwaysAvailable":true
				}
		},
		//霉菌来袭，困难重重
		{
			"aliases": [
				"DoNotPlantBeforeLine"
			],
            "objclass": "MoldColonyChallengeProps",
            "objdata": {
                "Locations": "RTID(nothingness@CurrentLevel)"
            }
        },
        {
            "aliases": [
                "nothingness"
            ],
            "objclass": "BoardGridMapProps",
            "objdata": {
                "Values": [
                    [1,1,1,1,1,1,1,1,1],
                    [0,0,0,0,0,0,0,0,0],
                    [1,1,1,1,1,1,1,1,1],
                    [0,0,0,0,0,0,0,0,0],
                    [1,1,1,1,1,1,1,1,1]
                ]
            }
        },
		//限制场上最多种植6株植物
		{
            "aliases": [
                "SimultaneousPlants"
            ],
            "objclass": "StarChallengeSimultaneousPlantsProps",
            "objdata": {
                "MaximumPlants": 6
            }
        },
		//保护危险中的植物
		{
			"aliases":[
				"ProtectThePlant"
			],
			"objclass":"ProtectThePlantChallengeProperties",
			"objdata":{
				"MustProtectCount":2,//需要保护的数量
				"Plants":[
				{
					"Level":-1,
					"GridX":4,
					"GridY":0,
					"PlantType":"potatomine"
				},
				{
					"Level":-1,
					"GridX":4,
					"GridY":2,
					"PlantType":"potatomine"
				},
				{
					"Level":-1,
					"GridX":4,
					"GridY":4,
					"PlantType":"potatomine"
				}
				]
			}
		},
		//花坛
		{
			"aliases": [
				"ZombieDistance"
			],
			"objclass": "StarChallengeZombieDistanceProps",
			"objdata": {
				"TargetDistance": 1.5
			}
		},
		
		//海盗登船
		{
			"aliases":[
				"Wave2RaidingPartyEvent0"
			],
			"objclass":"RaidingPartyZombieSpawnerProps",
			"objdata":{
				"GroupSize":1,
				"SwashbucklerCount":52,
				"TimeBetweenGroups":1
			}
		},
		//沙尘暴&暴风雪
		{
			"aliases": [
				"Wave9StormSpawn"
			],
			"objclass": "StormZombieSpawnerProps",
			"objdata": {
				"ColumnEnd": 7,
				"ColumnStart": 6,
				"GroupSize": 3,
				"TimeBetweenGroups": 1,
				"Type": "snowstorm",
				"Waves": "",
				"Zombies": [
				{
					"Type": "RTID(future_protector@ZombieTypes)"
				},
				{
					"Type": "RTID(future_protector@ZombieTypes)"
				},
				{
					"Type": "RTID(mech_cone@ZombieTypes)"
				}
				]
			}
		},
		{
			"aliases": [
				"Wave9StormSpawn"
			],
			"objclass": "StormZombieSpawnerProps",
			"objdata": {
				"ColumnEnd": 7,
				"ColumnStart": 6,
				"GroupSize": 3,
				"TimeBetweenGroups": 1,
				"Type": "sandstorm",
				"Waves": "",
				"Zombies": [
				{
					"Type": "RTID(future_protector@ZombieTypes)"
				},
				{
					"Type": "RTID(future_protector@ZombieTypes)"
				},
				{
					"Type": "RTID(mech_cone@ZombieTypes)"
				}
				]
			}
		},
		
		//固定位置生墓碑
		{
            "aliases": [
                "Wave4GravestoneEvent0"
            ],
            "objclass": "SpawnGravestonesWaveActionProps",
            "objdata": {
                "GravestonePool": [
                    {
                        "Count": 5,
                        "Type": "RTID(kongfu_rack_torch@GridItemTypes)"
                    }
                ],
                "SpawnEffectAnimID": "POPANIM_EFFECTS_TOMBSTONE_DARK_SPAWN_EFFECT",
                "SpawnPositionsPool": [
                    {"mX": 6, "mY": 0},
                    {"mX": 6, "mY": 1},
                    {"mX": 6, "mY": 2},
                    {"mX": 6, "mY": 3},
                    {"mX": 6, "mY": 4}
                ],
                "SpawnSoundID": "Play_Zomb_Egypt_TombRaiser_Grave_Rise",
                "maxX": 8,
                "minX": 3
            }
        },
		
		//随机位置生墓碑
		{
			"aliases":[
				"Wave0GravestoneSpawn0"
			],
			"objclass":"GravestoneProperties",
			"objdata":{
				"GravestoneCount":5,
				"SpawnColumnEnd":9,
				"Type":"RTID(gravestone_dark@GridItemTypes)",
				"SpawnColumnStart":6
			}
		},
		//僵王导弹突袭
		{
            "aliases": [
                "WaveMissileEvent"
            ],
            "objclass": "MissileLocateWaveActionProps",
            "objdata": {
                "MissileCount": 3,
                "MissileFallCountdown": 0,
                "WaveStartMessage": "",
                "ProjectileType": "zomboss_missile_dark",
                "Locations": [
                    {
                        "mX": 3,
                        "mY": 0
                    },
                    {
                        "mX": 4,
                        "mY": 1
                    },
                    {
                        "mX": 3,
                        "mY": 2
                    },
                    {
                        "mX": 3,
                        "mY": 3
                    },
                    {
                        "mX": 4,
                        "mY": 4
                    }
                ]
            }
        },
		
		//关卡目标&描述
		{
			"aliases":["BeatTheLevel"],
			"objclass": "StarChallengeBeatTheLevelProps",
			"objdata": {
				"Descriptions": [
					"Made by Birdscraft"
				] 
			}
		},
		
		//巨浪沙滩初始潮水设置
		{
			"aliases": [
				"Tide0"
			],
			"objclass": "TideProperties",
			"objdata": {
				"StartingWaveLocation": 1
			}
		},
		
		//巨浪沙滩潮水变化
		{
			"aliases": [
				"Wave9TidalChangeEvent0"
			],
			"objclass": "TidalChangeWaveActionProps",
			"objdata": {
				"TidalChange": {
					"ChangeAmount": 5,
					"ChangeType": "absolute"
				}
			}
		},
		//海盗港湾甲板设置
		{
			"aliases": [
				"PiratePlanks"
			],
			"objclass": "PiratePlankProperties",
			"objdata": {
				"PlankRows": [0,1,2,3]
			}
		},
		//退潮出僵尸
		{
			"aliases":[
				"Wave3LowTideEvent0"
			],
			"objclass":"BeachStageEventZombieSpawnerProps",
			"objdata":{
				"ColumnEnd":8,
				"ColumnStart":6,
				"GroupSize":"1",
				"TimeBeforeFullSpawn":"0.1",
				"TimeBetweenGroups":"0",
				"WaveStartMessage":"[WARNING_LOW_TIDE]", 
				"ZombieCount":"3",
				"ZombieName":"beach_imp"
			}
		},
		//浮漂
		{
			"aliases": [
				"SliderPlacement"
			],
			"objclass": "InitialGridItemProperties",
			"objdata": {
				"InitialGridItemPlacements": [
				{
					"GridX": 2,
					"GridY": 0,
					"TypeName": "slider_up"
				},
				{
					"GridX": 2,
					"GridY": 1,
					"TypeName": "slider_up"
				},
				{
					"GridX": 2,
					"GridY": 2,
					"TypeName": "slider_up"
				},
				{
					"GridX": 2,
					"GridY": 3,
					"TypeName": "slider_down"
				},
				{
					"GridX": 2,
					"GridY": 4,
					"TypeName": "slider_down"
				}
				]
			}
		},
		
		//失落黄金砖
		{
			"aliases":[
				"GoldTiles"
			],
			"objclass":"InitialGridItemProperties",
			"objdata":{
				"InitialGridItemPlacements":[
					{
						"GridX":0,
						"GridY":1,
						"TypeName":"goldtile"
					},
					{
						"GridX":0,
						"GridY":3,
						"TypeName":"goldtile"
					}
				]
			}
		},
		
		//失落飞行员降落伞
		{
			"aliases":[
				"Wave10ParachuteRainEvent0"
			],
			"objclass":"ParachuteRainZombieSpawnerProps",
			"objdata":{
				"ColumnEnd":7, 
				"ColumnStart":4, 
				"GroupSize":1, 
				"SpiderCount":11, 
				"SpiderZombieName":"lostcity_lostpilot",
				"TimeBeforeFullSpawn":6,
				"TimeBetweenGroups":0.5,
				"WaveStartMessage":"[WARNING_PARACHUTERAIN]",
				"ZombieFallTime":"12"
			}
		},
		
		//未来机械小鬼空降
		
		{
            "aliases": [
                "Wave2SpiderRainEvent0"
			],
            "objclass": "SpiderRainZombieSpawnerProps",
            "objdata": {
                "ColumnEnd": 7,
                "ColumnStart": 5,
                "GroupSize": 1,
                "SpiderCount": 6,
                "SpiderZombieName": "future_imp",
                "TimeBeforeFullSpawn": 1,
                "TimeBetweenGroups": 0.2,
                "WaveStartMessage": "[WARNING_SPIDERRAIN]",
                "ZombieFallTime": 1.5
            }
        },
		
		//未来僵尸空降
		
		{
            "aliases": [
                "Wave6JetpackSpawnEvent0"
            ],
            "objclass": "SpawnZombiesFromGroundSpawnerProps",
            "objdata": {
                "ColumnEnd": 8,
                "ColumnStart": 4,
                "WaveStartMessage": "[WARNING_JETPACKRAIN]",
                "Zombies": [
                    {
                        "Type": "RTID(future_jetpack@ZombieTypes)"
                    },
					{
                        "Type": "RTID(future_jetpack@ZombieTypes)"
                    },
					{
                        "Type": "RTID(future_jetpack_disco@ZombieTypes)"
                    },
					{
                        "Type": "RTID(future_jetpack_disco@ZombieTypes)"
                    },
					{
                        "Type": "RTID(future_jetpack_veteran@ZombieTypes)"
                    },
					{
                        "Type": "RTID(future_imp@ZombieTypes)"
                    },
                    {
                        "Type": "RTID(future_imp@ZombieTypes)"
                    }
                ]
            }
        },
		//时空裂缝
		{
			"objclass": "PortalType",
			"aliases": [
				"butterfly20"
			],
			"objdata": {
				"Properties": "RTID(butterflyprops@CurrentLevel)"
			}
		},
			{
			"objclass": "PortalProps",
			"aliases": [
				"butterflyprops"
			],
			"objdata": {
				"TimeBeforeFirst": {
				"Min": 20,
				"Max": 20
				},
				"TimeBetween": {
				"Min": 1,
				"Max": 1
				},
				"Scale": 150,//大小
				"World": "summer_pockets",
				"ZombieTypesToSpawn": [
					{
						"Weight": 1000,
						"ZombieTypeName": "future_imp"
					},
				]//有多少生成多少，weight基本没用
			}
		},
		{
			"aliases": [
				"butterfly20"
			],
			"objclass": "SpawnModernPortalsWaveActionProps",
			"objdata": {
				"PortalColumn": 8,
				"PortalRow": "4",
				"PortalType": "RTID(butterfly20@CurrentLevel)",
				"SpawnEffectAnimID": "",
				"SpawnSoundID": ""
			}
		}
		//信息红条
		{
			"aliases": [
				"Warning0"
			],
			"objclass": "WaveWarningProps",
			"objdata":{
				//字体颜色
				"LabelR":255,
				"LabelG":100,
				"LabelB":0,
				"LabelAInit":7,
				//描边颜色
				"OutlineR":0,
				"OutlineG":0,
				"OutlineB":0,
				"String":"La La Xu",
				"StringMultiLanguage":{
					"en":"La La Xu",
					"zh":"皇城"
				},
				"Sound":"HugeWave",
				"LabelTargetScale":0.5,
				"Duration":3,
				"InitTime":0.33
			}
		},
		//提示
		{
			"aliases": [
				"Tips"
			],
			"objclass": "TutorialLabelProperties",
			"objdata":{
				"String":"df",
				"StringMultiLanguage":{
					"en":"TTTT",
					"zh":"鸟？？？"
				},
				"LifeSpan":5
			}
		},
		
}

```