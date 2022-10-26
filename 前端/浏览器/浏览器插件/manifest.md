# manifest.json 配置文件

> manifest.json是扩展的配置文件，指明了扩展的各种信息。

一个manifest.json格式如下：

```js

{
    // 必须的字段3个
    "name": "MyExtension", // 扩展名称
    "version": "1.0", // 版本。由1到4个整数构成。多个整数间用"."隔开
    "manifest_version": 2, // manifest文件版本号。Chrome18开始必须为2
    // 建议提供的字段3个

    "description": "",   // 描述。132个字符以内

    "icons": {

       "16": "image/icon-16.png",

       "48": "image/icon-48.png",

       "128": "image/icon-128.png"

    }, //扩展图标。推荐大小16，48，128

    "default_locale": "en", // 国际化

    // 以下字段多选一，或者都不提供

    "browser_action": {
       "default_icon": "image/icon-128.png",
        "default_title": "My Test",
       "default_popup": "html/browser.html"
    }, //地址栏右侧图标管理。含图标及弹出页面的设置等
    "page_action": {
        "default_icon": "image/icon-48.png",
        "default_title": "My Test",
        "default_popup": "html/page.html"
    }, //地址栏最后附加图标。含图标及行为等
    "theme": {}, // 主题，用于更改整个浏览器的外观
    "app": {}, // 指定扩展需要跳转到的URL
    // 根据需要提供
    "background": {
       "scripts": [
           "lib/jquery-3.3.1.min.js",
           "js/background.js"

       ] ,

        "page":"html/background.html"

    }, // 指定扩展进程的background运行环境

    "chrome_url_overrides": {

       "pageToOverride": "html/overrides.html"

    }, //替换页面。详见注释1
    "content_scripts": [{
      "matches": ["https://www.baidu.com/*"],
      "css": ["css/mystyles.css"],
      "js": ["lib/jquery-3.3.1.min.js", "js/content.js"]
    }], // 指定在web页面运行的脚本。详见注释2

    "content_security_policy": "",  // 安全策略

    "file_browser_handlers": [],   

    "homepage_url": "http://xxx", // 扩展的官方主页

    "incognito": "spanning", // 或"split"。详见注释3

    "intents": {}, // 用户操作意图描述

    "key": "", // 扩展唯一标识。不需要人为指定

    "minimum_chrome_version": "1.0", // 扩展所需chrome的最小版本

    "nacl_modules": [],  // 消息与本地处理模块映射

    "offline_enabled": true, // 是否允许脱机运行

    "omnibox": {

       "keyword": "myKey"

    }, //ominbox即地址栏。用于响应地址栏的输入事件

    "options_page": "aFile.html", // 选项页。用于在扩展管理页面跳转到选项设置

    "permissions": [

       "https://www.baidu.com/*",

       "background",

       "tabs"

    ], //权限。详见注释4

    "plugins": [{

       "path": "extension_plugin.dll",

       "public": true

    }], // 扩展。可调用第三方扩展

    "requirements": {}, // 指定所需要的特殊技术。目前只支持"3D"

    "update_url": "http://path/to/updateInfo.xml",   // 自动升级

    "web_accessible_resources": [] // 指定资源路径，为String数组

}
```

## 注释

### chrome_url_overrides

> 替换页面。用于将原定显示的页面替换为自定义的页面。其取值有4个：

①   pageToOverride：页面。

②   bookmarks：书签。

③   history：历史。

④   newtab：新标签页。

### content_scripts

> 指定要向Web页面内注入的脚本。可注入多个css与js。

扩展本身的运行环境为background，而background与web页面的运行环境是相互独立的。若希望在扩展中对web页面进行修改，那就需要使用content_scripts。

content_scripts运行在一个隔离环境中，即与background和web页面运行环境都独立。但content_scripts共享了web页面的DOM，所以content_scripts可以对web页面DOM进行操作。然而共享仅限于DOM，不包括任何js变量与函数。同理，web页面的js也不能访问content_scripts中的js变量与函数。于是，二者引入的库也不能共享，各自使用各自的库。

借助事件的监听与触发，content_scripts可以与web页面的js进行通信。

content_scripts格式如下：

```js

"content_scripts": [{

      "matches": ["https://www.baidu.com/*"],

      "css": ["mystyles.css"],

      "js": ["lib/jquery-3.3.1.min.js", "js/content.js"],

      "run_at": "document_idle"

}],
```

其数组元素的字段有：

①   matches：String数组，必须。定义content_scripts对哪些页面生效。其规则符合permissions的模式匹配。

②   css：String数组，可选。定义哪些css文件在web页面DOM创建前注入到web页面中。

③   js：String数组，可选。定义哪些js文件注入到web页面中。其js文件的注入顺序与数组中定义的顺序相同。至于这些js与web页面中所定义js的顺序关系，取决于run_at字段。

④   run_at：String，可选。定义content_scripts的注入时机，从而影响到js与web页面所定义js的顺序关系。取值有：

·document_start：所有css加载完毕，但DOM尚未创建时。

·document_end：DOM创建完成，但图片及frame等子资源尚未加载时。

·document_idle：document_end之后，window.onload之前。

      默认是document_idle，也就是content_scripts的js都罗列在web页面的js之后。

⑤   all_frames：boolean，可选。是否运行在页面所有的frame中。若为false，则只运行在最上层的frame中。默认为false。

⑥   include_globs：String数组，可选。用于规定页面匹配的白名单。一个URL，必须同时满足：匹配matches，匹配include_globs白名单，不匹配exclude_globs黑名单这三个条件才可以。注意include_globs和exclude_globs中的匹配语法与permissions和matches所用的匹配模式不同。

⑦   exclude_globs：String数组，可选。用于规定页面匹配的黑名单。同⑥。

### incognito

> 取值为：”spanning”/“split”。指定当浏览器处于隐身模式下时，扩展如何响应。默认为”spanning”。

①   spanning：扩展会运行在共享进程中。标签页的事件和消息会发送给这个进程，来源通过incognito标志区分。

②   split：扩展运行在独立的进程中。每个进程都只能看到自己的事件和消息，各进程间不能通信。

### permissions

>扩展所需要的权限。permissions是一个String数组，每一个权限都使用String来表示。某些权限在安装前会告知用户。

特别注意permissions中若有不可识别的配置项，那么所有permissions配置项都会失效，即使其他的permissions配置项是正确的。

permissions部分常用值如下：

①   模式配匹：用于指定扩展会在哪些URL中生效。例如：

http://*/*：匹配任何http协议的URL。

*://*/*：匹配任何http/https协议的URL。

<http://127.0.0.1/*：匹配任何本地请求URL>。

<https://www.baidu.com/*：匹配https://www.baidu.com/开头的所有URL>。

<all_urls>：匹配所有url。

使用模式匹配时，直接将匹配规则字符串添加到permissions即可。

可添加多个匹配规则字符串，匹配效果是这些规则的并集。

若不添加匹配规则字符串，则默认对所有URL生效。

②   background：启用扩展后端环境。即在浏览器运行期始终运行，与单个页面无关。可以在这里调用浏览器的API，通常在这里进行扩展主要逻辑的开发。配合manifest.json的background字段使用。

③   bookmarks：启用书签权限。

④   contextMenus：启用右键菜单权限。可针对不同文档单独定制。

⑤   cookies：启用cookies权限。

⑥   experimental：启用chrome的实验功能API。

⑦   geolocation：允许扩展使用HTML5的地理位置API，无论用户是否有该权限。

⑧   history：启用history权限。

⑨   idle：启用延迟加载。

⑩   management：启用管理权限。用于管理已安装和正在运行的扩展。

⑪   notifications：启用桌面通知权限。不同平台下表现不同，但往往都是一个小弹窗。

⑫   tabs：启用标签权限。用于管理chrome浏览器的标签栏，例如创建，修改，重新排列标签等，以及修改chrome窗体。

实际上，大部分的chrome.tabs.API不需要任何权限就可以使用。tab的url、title和favIconUrl属性必须具有tabs权限才可以使用；tabs.captureVisibleTab必须有<all_urls>或activeTab权限才能使用。

⑬   activeTab：启用活动标签权限。通常来说，tabs更多是用于操作tab标签和窗体属性的。对那些可以与当前web页面交互的API，往往都需要activeTab权限。例如，tabs.executeScript（向页面临时注入js并执行）和tabs.insertCSS（向页面临时注入css并执行），执行浏览器按钮，执行页面按钮，执行右键菜单项，通过命令API执行键盘快捷键等。
