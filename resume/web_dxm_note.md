##### 1.事件执行顺序

```javascript
// 事件捕获 事件处理 事件冒泡 --- 主要看理解
```

##### 2.按钮 

```javascript
// 点击按钮发送请求,按钮不能被点击并显示60s倒计时，时间结束恢复初始状态
```

##### 3.防抖和节流

```javascript
/*
 例如：
    防抖：防抖是将多次执行变为最后一次执行 => 立即执行和延迟执行
    节流：多次执行变为在规定时间内只执行一次
    以input输入框为例分别做到防抖和节流,时间600ms
*/
```

##### 4.匹配最长字符串

```javascript
/*
	10w条字符串、匹配出共同部分最长的两个字符串。
	例如：["abcrweuie","uwfbcrw","arweuie"] 最长的就是 "rweuie"
	主要考的是计算机思维、代码优化。
*/
var sumStr="0123456789zxcvbnmasdfghjklqwertyuiopAZXCVBNMLKJHGFDSQWERTYUIOP-_",listDetail = [],lisrDetailOnce = [];
console.log("程序开始...");
// 生成 1w 条字符串
var strList = (function(sumStr){
	var sum = 10000 , list = []; // sum 控制数量
	for(var i=0;i<sum;i++){
		var str = "";
		for(var j=0;j<Math.ceil(Math.random()*15);j++){
			str += sumStr[Math.floor(Math.random()*64)];
		}
		list.push(str);
	}
	return list;
})(sumStr);
console.log("数据已生成，共计"+strList.length+"条。\r\n开始排序...\r\n计时开始")
// 开始排序
console.time("x");
strList.forEach(e=>{
	var eLen;
	if(typeof e == "string"){ 
		eLen = e.length;
	}else{
		return listDetail.push(e);
	}
    var temp = [];
	for(var i = 0 ; i < eLen-1 ; i++){
		temp.push(e.slice(i,eLen))
	}
    for(var i = 0 ,len = eLen;  len != 1 ; len--){
		temp.push(e.slice(i,len))
	}
    listDetail.push(...new Set(temp));
})
lisrDetailOnce = [...new Set(listDetail)].sort((a,b)=>b.length - a.length);
console.log("数组去重结束\r\n开始查找...");
// console.log(lisrDetailOnce,listDetail);
var endLen = lisrDetailOnce.length;
var i_str = "";
var returnData = [];
for(var i=0;i<endLen;i++){
	i_str = lisrDetailOnce[i];
	var state  = listDetail.indexOf(i_str) != listDetail.lastIndexOf(i_str);
	if(state){
		returnData.push(i_str);
		break;
	}else{
		// console.log(`${state},${i_str},${listDetail.indexOf(i_str)} == ${listDetail.lastIndexOf(i_str)}`) 不符合条件
	}
}
console.timeEnd("x");
console.log("查找结束-->"+returnData.join("\r\n"));

```

##### 5.先把前几题做了