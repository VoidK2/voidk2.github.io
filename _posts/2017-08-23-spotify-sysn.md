---
layout:     post
title:      如何将网易云歌单同步到Spotify
subtitle:   Spotify is the best!
date:       2017-08-23
author:     zhangzexin
header-img: img/home-bg.jpg
catalog: true
tags:
       - spotify
       - python
---
## 为什么要用Spotify?
1、网易云的弊端 
> * 网易云大量用户涌入，内容质量降低，有目共睹。
> * 成也评论，败也评论。带上社交属性的音乐还是音乐吗？
> * 我不喜欢网易云的一系列行为。

2、Spotify的优势
> * 全球用户量最大的流媒体播放平台
> * 曲库丰富，最先吵起来音乐版权的美国乡村歌手Taylor Swift的音乐，在这个平台上也是免费在线播放的。
> * 版权 
> * ui设计优秀，简洁便利。黑色背景配合amoled屏幕很爽。
> * 推送比网易云精准，更合口味。

## 用Spotify之后，我网易云的歌单怎么办？
这就是教程里要说的了，如果你听英文歌多，那就没问题了，因为spotify没有英文曲库

## 如何将网易云音乐的歌单同步到spotify？

步骤
> * 获取歌单的id：从浏览器进入到你的歌单，复制地址栏中”music.163.com/#/playlist?id=”后面的数字。 
> * 到后面下载自己可以用的py文件，请确保电脑上已经正确安装Python。随后，双击.py文件，输入步骤一获得的歌单id。
> * 运行.py文件后，在与源码同一文件夹里会出现一个.txt文件，这就是生成的歌单信息了。
> * 点开[这里](http://spotlistr.herokuapp.com/#/search/textbox)并粘贴.txt中的全部内容，等待其自动识别并创建歌单。（自行登录spotify帐号）
注意 
一个歌单在一千首以内，api只提供这么多。 
部分音乐因为版权问题，无法添加。（这种情况比较少）

## 源码 python2
```python
import json
import sys
from urllib import urlopen

enterId = 'Enter the playlist id (Enter ? to get help):'
help = 'To get the id of the playlist, go to the page\
of it and look at the address bar.\
 \nPlaylist id is the numbers after \
 "http://music.163.com/#/playlist?id="'
errRetrive = 'No data retrived. \nPlease check the playlist id again.'

while 1:
	playlistId = raw_input(enterId)

	#change the playlistId variable 
	if playlistId == '?':
		print
	urladd = "http://music.163.com/api/playlist/detail?id="\
		+ str(playlistId) + "&updateTime=-1"
	# Your code where you can use urlopen
	response = urlopen(urladd).read()
	data = json.loads(response)

	output = ""

	if "result" not in data:
		print(errRetrive)
		print(help)
		continue

	tracks = data["result"]["tracks"]
	for track in tracks:
		trackName = track["name"]
		artist = track["artists"][0]["name"]
		output += trackName + ' - ' + artist + '\n'
	playlistName = data["result"]["name"]

	with open(playlistName + '.txt', 'w') as file:
		file.write(output.encode('utf8'))

	print('Success.\nCheck the directory of this file and find the .kgl file!')
```
##源码python3
```python
# -*- coding: utf-8 -*-
import json
import sys, io
from urllib.request import urlopen

enterId = 'Enter the playlist id (Enter ? to get help):'
help = 'To get the id of the playlist, go to the page\
of it and look at the address bar.\
 \nPlaylist id is the numbers after \
 "http://music.163.com/#/playlist?id="'
errRetrive = 'No data retrived. \nPlease check the playlist id again.'

while 1:
	playlistId = input(enterId)

	#change the playlistId variable 
	if playlistId == '?':
		print
	urladd = "http://music.163.com/api/playlist/detail?id="\
		+ str(playlistId) + "&updateTime=-1"
	# Your code where you can use urlopen
	with urlopen(urladd) as url:
		response = url.read().decode('utf-8')
	data = json.loads(response)

	output = ""

	if "result" not in data:
		print(errRetrive)
		print(help)
		continue

	tracks = data["result"]["tracks"]
	for track in tracks:
		trackName = track["name"]
		artist = track["artists"][0]["name"]
		output += trackName + ' - ' + artist + '\n'
	playlistName = data["result"]["name"]


	with open(playlistName+'.txt', 'w',encoding='utf-8') as file:
		file.write(output)

	print('Success.\nCheck the directory of this file and find the .kgl file!')
```
