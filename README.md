# esports-autocaster
For Tournament Organizers & Streamers.

ゲームのスコア・ラウンド・キル・クラッチなどの状態を JSON で取得し、
Advanced Scene Switcher（ASS） と OBS を自動制御するシステムです。

「キルが起きたらリプレイ」「フリーズタイムはピクチャーインピクチャー」など、
これまでディレクターの勘と反射神経で行っていた演出を、ルールと AI で再現します。

## 概要
eSports AutoCaster は、「ゲームの中で起きていること」 と 「スイッチャーの操作」 を接続し、
大会配信のシーン切替・リプレイ・ハイライト演出を自動化するフレームワークです。

## Features
### 01. Game-Aware Scene Switching
**ゲームイベント連動のシーン自動切替**
ゲームから送られてくる JSON を解析し、Advanced Scene Switcher（ASS）用のイベントに変換して自動切替を行います。
対応例：
* 
