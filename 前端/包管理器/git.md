# git

<https://git-scm.com/docs/pretty-formats>

首先肯用到的node的核心模块，不再多说了，具体使用方法可以看一下node.js官网。

```js
const exec = require('child_process').exec; //异步子进程
const execSync = require('child_process').execSync; //同步子进程
const fs = require('fs'); //文件读取模块
```

如果version.txt存在的话，将先读取里边的版本信息

```js
if (fs.existsSync(versionPath)) {
    versionStr = fs.readFileSync(versionPath).toString() + '\n';
}
```

根据版本信息是已存在commit，进行不同处理

```js

if (versionStr.indexOf(commit) != -1) {
    console.warn('\x1B[33m%s\x1b[0m', 'warming: 当前的git版本数据已经存在了!\n')
} else {
    let name = execSync('git show -s --format=%cn').toString().trim(); //姓名
    let email = execSync('git show -s --format=%ce').toString().trim(); //邮箱
    let date = new Date(execSync('git show -s --format=%cd').toString()); //日期
    let message = execSync('git show -s --format=%s').toString().trim(); //说明
    versionStr = `git:${commit}\n作者:${name}<${email}>\n日期:${date.getFullYear()+'-'+(date.getMonth()+1)+'-'+date.getDate()+' '+date.getHours()+':'+date.getMinutes()}\n说明:${message}\n${new Array(80).join('*')}\n${versionStr}`;
    fs.writeFileSync(versionPath, versionStr);
    // 写入版本信息之后，自动将版本信息提交到当前分支的git上
    if (autoPush) {
        execSync(`git commit ${versionPath} -m 自动提交版本信息`);
        execSync(`git push origin ${execSync('git rev-parse --abbrev-ref HEAD').toString().trim()}`);
    }
}
```

附上获取git指定信息方法（<https://git-scm.com/docs/pretty-formats>）

```sh
%H: commit hash
%h: 缩短的commit hash
%T: tree hash
%t: 缩短的 tree hash
%P: parent hashes
%p: 缩短的 parent hashes
%an: 作者名字
%aN: mailmap的作者名字 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
%ae: 作者邮箱
%aE: 作者邮箱 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
%ad: 日期 (--date= 制定的格式)
%aD: 日期, RFC2822格式
%ar: 日期, 相对格式(1 day ago)
%at: 日期, UNIX timestamp
%ai: 日期, ISO 8601 格式
%cn: 提交者名字
%cN: 提交者名字 (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
%ce: 提交者 email
%cE: 提交者 email (.mailmap对应，详情参照git-shortlog(1)或者git-blame(1))
%cd: 提交日期 (--date= 制定的格式)
%cD: 提交日期, RFC2822格式
%cr: 提交日期, 相对格式(1 day ago)
%ct: 提交日期, UNIX timestamp
%ci: 提交日期, ISO 8601 格式
%d: ref名称
%e: encoding
%s: commit信息标题
%f: sanitized subject line, suitable for a filename
%b: commit信息内容
%N: commit notes
%gD: reflog selector, e.g., refs/stash@{1}
%gd: shortened reflog selector, e.g., stash@{1}
%gs: reflog subject
%Cred: 切换到红色
%Cgreen: 切换到绿色
%Cblue: 切换到蓝色
%Creset: 重设颜色
%C(...): 制定颜色, as described in color.branch.* config option
%m: left, right or boundary mark
%n: 换行
%%: a raw %
%x00: print a byte from a hex code
%w([[,[,]]]): switch line wrapping, like the -w option of git-shortlog(1).
```

将version文件移植到打包文件中

```js

if (fs.existsSync(buildPath)) {
    fs.writeFileSync(`${buildPath}/${versionPath}`, fs.readFileSync(versionPath));
}
```

程序运行后，打印一个字母图案（显得高大上一些），附上一个可以根据输入的字母生成打印的字体的网站（<http://patorjk.com/software/taag/#p=testall&v=2&f=Graffiti&t=PERSAGY>）

```js
// 打包成功并且版本信息写入成功
console.info([
  "██████╗ ███████╗██████╗ ███████╗ █████╗  ██████╗██╗   ██╗",
  "██╔══██╗██╔════╝██╔══██╗██╔════╝██╔══██╗██╔════╝╚██╗ ██╔╝",
  "██████╔╝█████╗  ██████╔╝███████╗███████║██║  ███╗╚████╔╝ ",
  "██╔═══╝ ██╔══╝  ██╔══██╗╚════██║██╔══██║██║   ██║ ╚██╔╝  ",
  "██║     ███████╗██║  ██║███████║██║  ██║╚██████╔╝  ██║   ",
  "╚═╝     ╚══════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝ ╚═════╝   ╚═╝   ",
].join('\n'));
```

通过node 执行该代码之后，就可以得到预期效果了。

默认打印的颜色是白色，如果想根据成功失败警告打印不同的颜色的话，也可以加上对应的状态色彩哦。

```js

console.log('\x1B[36m%s\x1B[0m', info);  //cyan
console.log('\x1B[33m%s\x1b[0m:', path);  //yellow
var styles = {
    'bold'          : ['\x1B[1m',  '\x1B[22m'],
    'italic'        : ['\x1B[3m',  '\x1B[23m'],
    'underline'     : ['\x1B[4m',  '\x1B[24m'],
    'inverse'       : ['\x1B[7m',  '\x1B[27m'],
    'strikethrough' : ['\x1B[9m',  '\x1B[29m'],
    'white'         : ['\x1B[37m', '\x1B[39m'],
    'grey'          : ['\x1B[90m', '\x1B[39m'],
    'black'         : ['\x1B[30m', '\x1B[39m'],
    'blue'          : ['\x1B[34m', '\x1B[39m'],
    'cyan'          : ['\x1B[36m', '\x1B[39m'],
    'green'         : ['\x1B[32m', '\x1B[39m'],
    'magenta'       : ['\x1B[35m', '\x1B[39m'],
    'red'           : ['\x1B[31m', '\x1B[39m'],
    'yellow'        : ['\x1B[33m', '\x1B[39m'],
    'whiteBG'       : ['\x1B[47m', '\x1B[49m'],
    'greyBG'        : ['\x1B[49;5;8m', '\x1B[49m'],
    'blackBG'       : ['\x1B[40m', '\x1B[49m'],
    'blueBG'        : ['\x1B[44m', '\x1B[49m'],
    'cyanBG'        : ['\x1B[46m', '\x1B[49m'],
    'greenBG'       : ['\x1B[42m', '\x1B[49m'],
    'magentaBG'     : ['\x1B[45m', '\x1B[49m'],
    'redBG'         : ['\x1B[41m', '\x1B[49m'],
    'yellowBG'      : ['\x1B[43m', '\x1B[49m']
};
```

在package.json里的打包命令上加上执行该程序，（&&为同步执行，&为异步执行）

```js
"scripts": {
  "build": "vue-cli-service build --mode production --dest dist&&node version.js",
},
```

打包之后version.txt效果：
