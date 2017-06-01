# 设计基于HTML5的APP登录功能及安全调用接口的方式（原理篇）

### 你是否真的需要登录功能？

把这个问题放在最前面并不是灌水，而是真的见过很多并不需要登录的APP去做了登录功能，或者是并不需要强制登录的APP把登录作为启动页。

用户对你的APP一无所知，你就要求对方注册并登录，除非APP本身已经很有名气或者是用户有强需求，否则正常人应该会直接把它删掉。

比较温和的方式是将一些并不需要登录，但可以给用户带来帮助的东西，第一时间展现给他们，让他们产生兴趣，再在合适的时机引导他们注册（比如使用需要使用更高级的功能，或用户需要收藏某个喜欢的信息时）。

### 登录和注册要足够简单

这是小小的手机端，用再好的输入法，打字也是不方便的，所以别把登录页设计得需要填很多东西。如果有可能的话，只填手机号，让用户收到短信验证码就完成注册是最好不过的了。想获得更多信息？想想大公司的APP是怎么做的，他们会告诉用户，现在的个人资料完善程度是30%，如果想获得更多积分，你需要填完。

tips:如果你想发布在Appstore并且同时包含注册功能，那么注册页面必须做一个用户许可协议的链接，否则有可能通不过审核。

### 实现登录后的session有几种方式？

#### APP当浏览器用，直接载入远程页面

这种情况是很多偷懒的程序员或者傻X的老板选择的方式，因为做起来实在太快。如果本身网站是响应式布局，那么很有可能不需要做什么更改，就只要在开发时打开首页就好了，这样Hybird的APP外壳就纯粹成为了一个浏览器。

但比起这样做带来的无数缺点来，开发速度快的优点几乎可以忽略不计。

首先，在网络环境不佳时，纯大白页，用户体验0；

然后，CSS和JS等资源不在本地，需要远程载入，如果使用了bootstrap之类的框架，那用户为了开一下APP而耗费的流量真是令人感动；

再然后，网页里常用的jquery，在手机的webview里速度并不理想，而如果是非ajax的网页那就更糟心了，每次操作都要跳转和页面渲染，要让人把它当成APP那实在是笑话。

再再然后，这样的所谓APP，要通过Appstore的审查，那是做梦的（除非审核员当天闹肚子严重，拿着纸巾奔向厕所前误点了通过……），苹果的要求是，这得是APP，而不能是某个网站做成APP的样子，那样的情况适合做Web APP。而据我所知，国内几个较大的Android市场，这样的APP也是无法通过审核的。

#### 调用后端接口

这是个很好的时代，因为无论后端你是用Java、PHP，还是node.js，都可以通过xml、json来和APP通讯。遥想当年写服务端要自己写包结构，然后为了解决并发问题还折腾了半年IOCP模型，真心觉得现在太幸福了。

把刚才那个用APP当浏览器使的案例的所有缺点反过来看，就是这样做的优点，在优化完善的情况下体验接近原生，而且通讯流量极少，通过各种审核也是妥妥的。

tips:通过plus对象中的XMLHttpRequest来Get、Post远程的后端接口，或者使用Mui中封装好的AJAX相关函数。

插一段代码，我把mui的ajax又做了进一步的封装，对超时进行了自动重试，而对invalid\_token等情况也做相应处理：

    ;mui.web_query = function(func_url, params, onSuccess, onError, retry){
        var onSuccess = arguments[2]?arguments[2]:function(){};
        var onError = arguments[3]?arguments[3]:function(){};
        var retry = arguments[4]?arguments[4]:3;
        func_url = 'http://www.xxxxxx.com/ajax/?fn=' + func_url;
        mui.ajax(func_url, {
            data:params,
            dataType:'json',
            type:'post',
            timeout:3000,
            success:function(data){
                if(data.err === 'ok'){
                    onSuccess(data);
                }
                else{
                    onError(data.code);
                }
            },
            error:function(xhr,type,errorThrown){
                retry--;
                if(retry > 0) return mui.web_query(func_url, params, onSuccess, onError, retry);
                onError('FAILED_NETWORK');
            }
        })
    };
    var onError = function(errcode){
        switch(errcode){
        case 'FAILED_NETWORK':
            mui.toast('网络不佳');
            break;
        case 'INVALID_TOKEN':
            wv_login.show();
            break;
        default:
            console.log(errcode);
        }
    };
    var params = {per:10, pageno:coms_current_pageno};
    mui.web_query('get_com_list', params, onSuccess, onError, 3);

### 调用后端接口怎么样才安全？

#### 在APP中保存登录数据，每次调用接口时传输

程序员总能给自己找到偷懒的方法，有的程序为了省事，会在用户登录后，直接把用户名和密码保存在本地，然后每次调用后端接口时作为参数传递。真省事儿啊！可这种方法简单就像拿着一袋子钱在路上边走边喊“快来抢我呀！快来抢我呀！”，一个小小的嗅探器就能把用户的密码拿到手，如果用户习惯在所有地方用一个密码，那么你闯大祸了，黑客通过撞库的方法能把用户的所有信息一锅端。

#### 登录时请求一次token，之后用token调用接口

这是比较安全的方式，用户在登录时，APP调用获取token的接口（比如http://api.abc.com/get_token/），用post将用户名和密码的摘要传递给服务器，然后服务器比对数据库中的用户信息，匹配则返回绑定该用户的token（这一般翻译为令牌，很直观的名字，一看就知道是有了这玩意，就会对你放行），而数据库中，在用户的token表中也同时插入了这个token相关的数据：这个token属于谁？这个token的有效期是多久？这个token当前登录的ip地址是？这个token对应的deviceid是？……

这样即便token被有心人截获，也不会造成太大的安全风险。因为没有用户名和密码，然后如果黑客通过这个token伪造用户请求，我们在服务器端接口被调用时就可以对发起请求的ip地址、user-agent之类的信息作比对，以防止伪造。再然后，如果token的有效期设得小，过一会儿它就过期了，除非黑客可以持续截获你的token，否则他只能干瞪眼。（插一句题外话：看到这里，是不是明白为什么不推荐在外面随便接入来历不明的wifi热点了？）

tips:token如何生成？ 可以根据用户的信息及一些随机信息（比如时间戳）再通过hash编码（比如md5、sha1等）生成唯一的编码。

tips:token的安全级别，取决于你的实际需求，所以如果不是涉及财产安全的领域，并不建议太严格（比如用户走着走着，3G换了个基站，闪断了一下IP地址变了，尼玛token过期了，这就属于为了不必要的安全丢了用户体验，当然如果变换的IP地址跨省的话还是应该验证一下的，想想QQ有时候会让填验证码就明白了）。

tips:接口在返回信息时，可以包含本次请求的状态，比如成功调用，那么result['status']可能就是'success'，而反之则是'error'，而如果是'error'，则result['errcode']中就可以包含错误的原因，比如errcode中是'invalid_token'就可以告诉APP这个token过期或无效，这时APP应弹出登录框或者用本地存储的用户名或密码再次请求token（用户选择“记住密码”，就应该在本地保存用户名和密码的摘要，方法见plus.storage的文档）。

再插点代码，基于plus.storage的用户信息类，注意：需要在plusReady之后再使用。

    ;function UserInfo(){
    };

    //清除登录信息
    UserInfo.clear = function(){
        plus.storage.removeItem('username');
        plus.storage.removeItem('password');
        plus.storage.removeItem('token');
    }

    //检查是否包含自动登录的信息
    UserInfo.auto_login = function(){
        var username = UserInfo.username();
        var pwd = UserInfo.password();
        if(!username || !pwd){
            return false;
        }
        return true;
    }

    //检查是否已登录
    UserInfo.has_login = function(){
        var username = UserInfo.username();
        var pwd = UserInfo.password();
        var token = UserInfo.token();
        if(!username || !pwd || !token){
            return false;
        }
        return true;
    };

    UserInfo.username = function(){
        if(arguments.length == 0){
            return plus.storage.getItem('username');        
        }
        if(arguments[0] === ''){
            plus.storage.removeItem('username');
            return;
        }
        plus.storage.setItem('username', arguments[0]);
    };

    UserInfo.password = function(){
        if(arguments.length == 0){
            return plus.storage.getItem('password');        
        }
        if(arguments[0] === ''){
            plus.storage.removeItem('password');
            return;
        }
        plus.storage.setItem('password', arguments[0]);
    };

    UserInfo.token = function(){
        if(arguments.length == 0){
            return plus.storage.getItem('token');       
        }
        if(arguments[0] === ''){
            plus.storage.removeItem('token');
            return;
        }
        plus.storage.setItem('token', arguments[0]);
    };

这样当用户启动APP或使用了需要登录才能使用的功能时，就可以使用UserInfo.has_login()来判断是否已经登录，如果已登录，则使用UserInfo.token()来获取到token数据，作为参数调用远程的后端接口。

    if(UserInfo.has_login()){
        //打开需要展示给用户的页面，或者是调用远端接口
    }
    else{
        wv_login.show('slide-in-up');   //从底部向上滑出登录页面
    }

在登录页面中，用户输入了用户名和密码后，并点击了”登录“按钮，我们下一步做什么？再插段代码（注意：此处使用的是我刚才代码中扩展的web_query函数，你也可以直接使用mui的ajax）：

    function get_pwd_hash(pwd){
        var salt = 'hbuilder';  //此处的salt是为了避免黑客撞库，而在md5之前对原文做一定的变形，可以设为自己喜欢的，只要和服务器验证时的salt一致即可。
        return md5(salt + pwd); //此处假设你已经引用了md5相关的库，比如github上的JavaScript-MD5
    }

    //这里假设你已经通过DOM操作获取到了用户名和密码，分别保存在username和password变量中。
    var username = xxx;
    var password = xxx;
    var pwd_hash = get_pwd_hash(password);

    var onSuccess = function(data){
        UserInfo.username(username);
        UserInfo.password(pwd_hash);
        UserInfo.token(data.token); //把获取到的token保存到storage中
        var wc = plus.webview.currentWebview();
        wc.hide('slide-out-bottom');    //此处假设是隐藏登录页回到之前的页面，实际你也可以干点儿别的
    }

    var onError = function(errcode){
        switch(errcode){
        case 'INCORRECT_PASSWORD':
            mui.toast('密码不正确');
            break;
        case 'USER_NOT_EXISTS':
            mui.toast('用户尚未注册');
            break;
        }
    }

    mui.web_query('get_token', {username:username,password:pwd_hash}, onSuccess, onError, 3);

#### 更安全一点，获取token通过SSL

刚才的方法，机智一点儿的读者大概会心存疑虑：那获取token时不还是得明文传输一次密码吗？

是的，你可以将这个获取token的地址，用SSL来保护（比如https://api.abc.com/get_token/），这样黑客即使截了包，一时半会儿也解不出什么信息。

SSL证书的获取渠道很多，我相信你总有办法查到，所以不废话了。不过话说namecheap上的SSL证书比godaddy的要便宜得多……（这是吐槽）

tips:前段时间OpenSSL漏洞让很多服务器遭殃，所以如果自己搭服务器，一定记得装补丁。

tips:可以把所有接口都弄成SSL的吗？可以。但会拖慢服务器，如果是配置并不自信的VPS，建议不折腾。

#### 还要更更安全（这标题真省事）

还记得刚才APP向服务器请求token时，可以加入的用户信息吗？比如用户的设备deviceid。

如果我们在调用接口时，还附带一个当前时间戳参数timestamp，同时，用deviceid和这个时间戳再生成一个参数sign，比如 md5(deviceid timestamp token)这样的形式。而服务端首先验证一下参数中的时间戳与当前服务器时间是否一致（误差保持在合理范围内即可，比如5分钟），然后根据用户保存在服务器中的deviceid来对参数中的时间戳进行相同的变形，验证是否匹配，那便自然“更更安全”了。

tips:如果对整个调用请求中的参数进行排序，再以deviceid和timestamp加上排序后的参数来对整个调用生成1个sign，黑客即使截获sign，不同的时间点、参数请求所使用的sign也是不同的，难以伪造，自然会更安全。当然，写起来也更费事。

tips:明白了原理，整个验证过程是可以根据自己的需求改造的。
