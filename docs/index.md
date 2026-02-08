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

---
sidebar_position: 0
---

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
*"objclass"*可以看作一个函数，*"objdata"*可以看作自变量

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

接下来添加*"WaveManagerProps"*，*"aliases"*相当于函数名
其中：
	*"SuppressFlagZombie"*为是否删除旗帜僵尸
	*"FlagWaveInterval"*，*"WaveCount"*共同决定旗帜数（非波数）
	*"Waves"*为波，其中有几个*[]*就有几波，其中：
		*[]*空对象即为空波
		
这里定义函数*"Wave0"*举例（当然正式写关都要定义）
	*"aliases"*为函数名，可任意填写
	*"AdditionalPlantfood"*为本波叶绿素数量
	*"Zombies"*中列举了本波出场的僵尸

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
					["RTID(Wave0@CurrentLevel)","RTID(Wave0GravestoneEvent1@CurrentLevel)"],
					["RTID(Wave1@CurrentLevel)"],
					["RTID(Wave2@CurrentLevel)"],
					["RTID(Wave3@CurrentLevel)"],
					["RTID(Wave4@CurrentLevel)"],
					[],
					["RTID(Wave5@CurrentLevel)"],
					["RTID(Wave6@CurrentLevel)","RTID(butterfly2@CurrentLevel)"],
					["RTID(Wave7@CurrentLevel)"],
					[],
					[],
					["RTID(Wave8@CurrentLevel)"],
					[],
					["RTID(Wave9@CurrentLevel)","RTID(butterfly4@CurrentLevel)"],
					["RTID(Wave10@CurrentLevel)"]
				]
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