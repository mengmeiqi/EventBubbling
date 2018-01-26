# EventBubbling
    事件冒泡问题
### 事件冒泡：
（1）从事件源开始，逐层往外传播
（2）无论是标准浏览器，还是非标准浏览器都有事件冒泡
（3）大多数事件都有事件冒泡
    onload，onfocus，onblur不冒泡
### 绑定事件的其他方法：
    （a）标准浏览器：
	oDiv1.addEventListener("click",function(){},false);
    Obj.addEventListener(type,function(){

    },false/true);
    有三个参数，第一个是绑定的事件，没有on，
    第二个是事件函数，第三个参数是布尔值，默认值是false；
    布尔值为false时是事件冒泡，布尔值为true时是事件捕获；
    事件冒泡和事件捕获是两个相反的过程；事件发生的过程是先捕获后冒泡；
    this指向是事件源：
	oDiv1.addEventListener("click", function(e){
	    alert("div1");
	    console.log(this);//this指的是oDiv1
	}
    阻止事件冒泡：
	e.stopPropagation();
    （b）非标准浏览器：
    obj.attachEvent(on+type,function(){
    });
    非标准浏览器只支持事件冒泡，不支持事件捕获；
    this指向是window，可以改变：
	oDiv1.attachEvent("onclick", function(){
            alert(1);
            window.event.cancelBubble = true;
	           alert(this === window);
        });
    阻止事件冒泡：
    window.event.cancelBubble = true;
    封装函数：
	function addEvent(elem, type, handler){
	    if(elem.addEventListener){//能力检测
	        elem.addEventListener(type, handler);
	    }else if(elem.attachEvent){
	        elem.attachEvent("on" + type, handler);
	    }else{
	        elem["on" + type] = handler;
	    }
	}

	事件冒泡能解决什么问题？
    事件委托/事件代理：
    利用事件冒泡把子元素的事件同意委托给父元素绑定
    （1）有很多子元素绑定一个事件，利用事件冒泡，
    10个Li，点击哪个就输出哪个的内容
    	var oNav = document.getElementById("nav");
    	/*var aLi = oNav.getElementsByTagName("li");
    	for(var i=0; i<aLi.length; i++){
    	    aLi[i].onclick = function(){
    	        console.log(this.innerHTML);
    	    };
    	}*/
    	oNav.onclick = function(e){
    	    var target = e.target || window.event.srcElement;
    	    console.log(target.innerHTML);
    	};
    （2）无缝滚动