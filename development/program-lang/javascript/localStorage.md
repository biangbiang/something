js本地存储解决方案(localStorage与userData)
==========================================

WEB应用的快速发展，是的本地存储一些数据也成为一种重要的需求，实现的方案也有很多，最普通的就是cookie了，大家也经常都用，但是cookie的缺点是显而易见的，其他的方案比如：IE6以上的userData，Firefox下面的globalStorage，以及Flash的本地存储，除了Flash之外，其他的几个都有一些兼容性的问题。

### sessionStorage与localStorage

Web Storage实际上由两部分组成：sessionStorage与localStorage。

sessionStorage用于本地存储一个会话（session）中的数据，这些数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储。

localStorage用于持久化的本地存储，除非主动删除数据，否则数据是永远不会过期的。

### userData

**语法：**

    XML	 <Prefix: CustomTag ID=sID STYLE="behavior:url('#default#userData')" />

    HTML	 <ELEMENT STYLE="behavior:url('#default#userData')" ID=sID>

    Scripting	 object .style.behavior = "url('#default#userData')"

    object .addBehavior ("#default#userData")

**属性:**

expires 设置或者获取 userData behavior 保存数据的失效日期。

XMLDocument 获取 XML 的引用。

**方法:**

getAttribute() 获取指定的属性值。

load(object) 从 userData 存储区载入存储的对象数据。

removeAttribute() 移除对象的指定属性。

save(object) 将对象数据存储到一个 userData 存储区。

setAttribute() 设置指定的属性值。

### localStorage

**方法：**

localStorage.getItem(key):获取指定key本地存储的值

localStorage.setItem(key,value)：将value存储到key字段

localStorage.removeItem(key):删除指定key本地存储的值

### 封装

    localData = {
            hname:location.hostname?location.hostname:'localStatus',
            isLocalStorage:window.localStorage?true:false,
            dataDom:null,

            initDom:function(){ //初始化userData
                if(!this.dataDom){
                    try{
                        this.dataDom = document.createElement('input');//这里使用hidden的input元素
                        this.dataDom.type = 'hidden';
                        this.dataDom.style.display = "none";
                        this.dataDom.addBehavior('#default#userData');//这是userData的语法
                        document.body.appendChild(this.dataDom);
                        var exDate = new Date();
                        exDate = exDate.getDate()+30;
                        this.dataDom.expires = exDate.toUTCString();//设定过期时间
                    }catch(ex){
                        return false;
                    }
                }
                return true;
            },
            set:function(key,value){
                if(this.isLocalStorage){
                    window.localStorage.setItem(key,value);
                }else{
                    if(this.initDom()){
                        this.dataDom.load(this.hname);
                        this.dataDom.setAttribute(key,value);
                        this.dataDom.save(this.hname)
                    }
                }
            },
            get:function(key){
                if(this.isLocalStorage){
                    return window.localStorage.getItem(key);
                }else{
                    if(this.initDom()){
                        this.dataDom.load(this.hname);
                        return this.dataDom.getAttribute(key);
                    }
                }
            },
            remove:function(key){
                if(this.isLocalStorage){
                    localStorage.removeItem(key);
                }else{
                    if(this.initDom()){
                        this.dataDom.load(this.hname);
                        this.dataDom.removeAttribute(key);
                        this.dataDom.save(this.hname)
                    }
                }
            }
        }

使用方法就很简单了，这节set,get,remove就好了。
