---
title: js中`!!`的小知识点
date: 2017-02-14 00:05:03
tags: javascript
---
今天在写网页音乐播放器的时候发现一个很有意思的小知识点。

在从API获取歌词的时候，我要判断通过服务器返回的数据歌词是否为空，通常我的做法会是这样写`if(lyr.lyric!=='undefined'）`，现在提供一种双重否定的写法。
示例代码如下：

    function getlyric(){
	var Sid=$('audio').attr('sid');
	var Ssid=$('audio').attr('ssid');
	$.post('http://api.jirengu.com/fm/getLyric.php',{ssid:Ssid,sid:Sid})
	.done(function(lyr){
		//解析歌词
		console.log(lyr); 
		var lyr=JSON.parse(lyr);
		//!!一般用来将后面的表达式强制转换为布尔类型的数据（boolean）
		if(!!lyr.lyric){
			$('.music-lyrics .lyric').empty();
			var line=lyr.lyric.split('\n');
			var timeReg=/\[\d{2}:\d{2}.\d{2}\]/g;//时间正则
			var result=[];
			//result是一个时间（秒）+歌词数组
			if(line!=""){
				for(var i in line){
					var time=line[i].match(timeReg);//每组匹配时间，得到时间数组
					if(!time) continue;
					var value=line[i].replace(timeReg,"");//纯歌词
					for(var j in time){
						var t=time[j].slice(1,-1).split(':');
						var timeArr=parseInt(t[0],10)*60+parseFloat(t[1]);
						result.push([timeArr,value]);
					}
				}
			}

`!!lyr.lyric`就相当于`lyr.lyric=lyr.lyric||false`。当值为undifined和null时，用一个感叹号返回的都是true,用两个感叹号返回的就是false。所以两个感叹号的作用就在于，如果明确设置了变量的值（非null/undifined/0/""等值),结果就会根据变量的实际值来返回，如果没有设置，结果就会返回false。 

