# X-Frame-Options 响应头

注意: CSP Level 2 规范中的 frame-ancestors 指令会替代这个非标准的 header。CSP 的 frame-ancestors 会在 Gecko 4.0 中支持，但是并不会被所有浏览器支持。然而 X-Frame-Options 是个已广泛支持的非官方标准，可以和 CSP 结合使用。

X-Frame-Options HTTP 响应头是用来给浏览器指示允许一个页面可否在 <frame>, <iframe> 或者 <object> 中展现的标记。网站可以使用此功能，来确保自己网站的内容没有被嵌到别人的网站中去，也从而避免了点击劫持 (clickjacking) 的攻击。

### 使用 X-Frame-OptionsEDIT

X-Frame-Options 有三个值:

##### DENY

表示该页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。

##### SAMEORIGIN

表示该页面可以在相同域名页面的 frame 中展示。

##### ALLOW-FROM uri

表示该页面可以在指定来源的 frame 中展示。

换一句话说，如果设置为 DENY，不光在别人的网站 frame 嵌入时会无法加载，在同域名页面中同样会无法加载。另一方面，如果设置为 SAMEORIGIN，那么页面就可以在同域名页面的 frame 中嵌套。

### 配置 Apache

配置 Apache 在所有页面上发送 X-Frame-Options 响应头，需要把下面这行添加到 'site' 的配置中:

    Header always append X-Frame-Options SAMEORIGIN

### 配置 nginx

配置 nginx 发送 X-Frame-Options 响应头，把下面这行添加到 'http', 'server' 或者 'location' 的配置中:

    add_header X-Frame-Options SAMEORIGIN;

### 配置 IIS

配置 IIS 发送 X-Frame-Options 响应头，添加下面的配置到 Web.config 文件中:

    <system.webServer>
      ...

      <httpProtocol>
        <customHeaders>
          <add name="X-Frame-Options" value="SAMEORIGIN" />
        </customHeaders>
      </httpProtocol>

      ...
    </system.webServer>

### 结果

在 Firefox 尝试加载 frame 的内容时，如果 X-Frame-Options 响应头设置为禁止访问了，那么 Firefox 会用 about:blank 展现到 frame 中。也许从某种方面来讲的话，展示为错误消息会更好一点。

### 浏览器兼容性

    Desktop
    特性	Chrome	Firefox (Gecko)	Internet Explorer	Opera	Safari
    基础支持	4.1.249.1042	3.6.9 (1.9.2.9)	8.0	10.5	4.0
    ALLOW-FROM 支持	Not supported	18.0 (18.0) bug 690168	8.0?	?	Not supported WebKit bug 94836

    Mobile
    特性	Android	Chrome Android 版	Firefox 移动版 (Gecko)	IE 移动版	Opera 移动版	Safari 移动版
    基础支持	?	?	?	?	?	?
