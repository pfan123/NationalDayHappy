NationalDayHappy
================

国庆节快乐-只想对你说(微信版) 访问地址http://pingfan1990.sinaapp.com/festival/index.html

国庆节快乐-只想对你说(微信版)，祝所有亲爱的国庆节快乐,开心每一天!幸福一生!!!愿所有亲爱的一切都好！所有亲爱的都平安健康！幸福美好。

通过封装pingfan对象方法，调用对象方法实现页面相关特效，例日期特效、随机切换图片、主题、歌曲，歌词与与音乐同步，个人认为最大的亮点。

有一个问题必须特别注意：ios设备audio标签必须形成交互，才能播放。

为了形成表面上自动播放，则采取如下办法加，window.addEventListener('touchstart',function(){audio.play()},false);




		/*
			@author pingfan
			pingfan命名空间
		*/
		var pingfan=function(){
			return{
				$$:function(id){
					return (!id) ? null :  document.getElementById(id);
				},
				//参数url对象post的地址，param为传的参数 格式例 "fname=bill&lname=gates"	
				hostPost:function(url,param,callback){
					var xmlhttp;
					if (window.XMLHttpRequest){
					  xmlhttp=new XMLHttpRequest();
					  }
					else{
					  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
					  }
					xmlhttp.onreadystatechange=function()
					  {
						  if(xmlhttp.readyState==4){
						  
							  if (xmlhttp.status==0 || xmlhttp.status==200)
								{				
									if(callback){
										callback(xmlhttp.responseText);					
									}

								}else{
									alert('请求歌词出错，请刷新浏览器')
								}
						  }
					  }	
					xmlhttp.open("POST",url,true);
					xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded"); 
					xmlhttp.send(param);		
				},
				//阻止默认行为
				stopDefault:function(e){
					var e=e || window.event;
					if(e.preventDefault){
						e.preventDefault();
					}else{
						e.returnValue=false;
					}			
				},
				addClass:function(element,value){
					if(!element.className) {
						element.className = value;
						} else {
						var newClassName = element.className;
						newClassName += " ";
						newClassName += value;
						element.className = newClassName;
					}				
				},
				removeClass:function(obj,cn) {
					return obj.className = obj.className.replace(new RegExp("\\b"+cn+"\\b"),"");
					//同样，多个className之间的多个空格也会被视为一个
				},
				getIndex:function(elem,arr){
					arr=arr || [];
					for(var i=0,len=arr.length;i<len;i++){		
						if(arr[i]==elem){
								return i;
						}
					}
				},
				tab:function(elem,classname,callback1,callback2){
					var elem=elem || [];
					for(var i=elem.length;i--;){
						elem[i].onclick=function(e){
							var element=e.target ? e.target : e.srcElement;	
							for(var j=elem.length;j--;){
								elem[j].className='';
							}
							element.className=classname;
							var id=pingfan.getIndex(element,elem);
							if(id==0 && callback1){
								callback1();
							}
							if(id==1 && callback2){
								callback2();
							}
						}
					}
				},
				getUrlParam:function(name){
					var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
					var r = window.location.search.substr(1).match(reg);
					if (r!=null) return unescape(r[2]); return null;
				},
				//移动端没有hover鼠标伪类，js模拟hover
				hover:function(elem,className){
					var elem=elem || [];
					for(var i=elem.length;i--;){
						elem[i].addEventListener('touchstart',function(){this.className=className;},false);
						elem[i].addEventListener('touchend',function(){this.className='';},false);
					}
				},
				isWeiXin:function(){  
					var ua = window.navigator.userAgent.toLowerCase();  
					if(ua.match(/MicroMessenger/i) == 'micromessenger'){  
						return true;  
					}else{  
					   return false;  
					}  
				},
				toDouble:function(num){
					num>=10 ? num=''+num : num='0'+num ;
							return num;
				},
				daoTimer:function(year,month,day,hour, minute,seconds,elem){               
							var hour=hour || 0,
								minute=minute || 0,
								seconds=seconds || 0;                                
							var endTime=new Date();                              
									endTime.setFullYear(parseInt(year)),                               
									endTime.setMonth(parseInt(month)-1),                             
									endTime.setDate(parseInt(day)),                          
									endTime.setHours(parseInt(hour)),                           
									endTime.setMinutes(parseInt(minute)),                           
									endTime.setSeconds(parseInt(seconds));      
							setTime();
							setInterval(function(){
									setTime()
							},1000);        
							function setTime(){                                                
									var startTime=new Date();
									if(endTime.getTime()-startTime.getTime()<0){
										var lengthTime=parseInt((startTime.getTime()-endTime.getTime())/1000);
									}else{
										var lengthTime=parseInt((endTime.getTime()-startTime.getTime())/1000);
									}
									var     lSeconds=parseInt(lengthTime % 60),                                
											lMinute=parseInt((lengthTime/60)%60),         
											lHour=Math.floor((lengthTime/3600)%24),
											lDay=Math.floor(lengthTime/(24*3600));
									if(endTime.getTime()-startTime.getTime()<0){		
										elem.innerHTML='2014年国庆节已过去'+lDay+'天'+pingfan.toDouble(lHour)+'小时'+pingfan.toDouble(lMinute)+'分钟'+pingfan.toDouble(lSeconds)+'秒';									
									}else{
										elem.innerHTML='离2014年国庆节还有'+lDay+'天'+pingfan.toDouble(lHour)+'小时'+pingfan.toDouble(lMinute)+'分钟'+pingfan.toDouble(lSeconds)+'秒';
									}
							}
							
				},
				loadImages:function(sources,callback){
					var count=0,
					images={},
					imgNum=0;
					sources=sources || [];
					for(src in sources){
						images[src]=new Image();
						images[src].onload=function(){
							if(++count >= imgNum){
								callback(images);
							}
						}
						images[src].src=sources[src];
					}			
				},
				loading:function(callback){
					if(typeof document.onreadystatechange != "undefined"){
						document.addEventListener('readystatechange',function(){
							callback();						
						},false)
					}else{
						window.addEventListener('load',function(){
							callback();
						},false)
					}
				},
				timeAll:function(elem){
					if(elem){
					
						elem.currentTime !=0 ? elem.duration : 0;
					}
				}
			}
		}();
		

祝朋友们节日快乐，喜欢就点星、点赞，也希望各为大神指点、交流。
