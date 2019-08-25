# puppeteer

## 简介

利用 puppeteer，可以操纵页面。puppeteer是一个nodejs的库，支持调用Chrome的API来操纵Web，相比较Selenium或是PhantomJs,它最大的特点就是它可以操作Dom，可以完全在内存中进行模拟不打开浏览器，而且关键是这个是Chrome团队在维护。

## 功能

生成页面的截图和PDF。

抓取SPA并生成预先呈现的内容（即“SSR”）。

从网站抓取你需要的内容。

自动表单提交，UI测试，键盘输入等

创建一个最新的自动化测试环境。使用最新的JavaScript和浏览器功能，直接在最新版本的Chrome中运行测试。

捕获您的网站的时间线跟踪，以帮助诊断性能问题

## 安装

直接运行 'npm i puppeteer'

可能会出现的错误

```
ERROR: Failed to download Chromium r497674! Set "PUPPETEER_SKIP_CHROMIUM_DOWNLOAD" env variable to skip download.
npm WARN backendbrowser@1.0.0 No repository field.
npm WARN backendbrowser@1.0.0 No license field.

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! puppeteer@0.10.2 install: node install.js
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the puppeteer@0.10.2 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR! /Users/xxxx/.npm/_logs/2017-09-04T02_50_51_507Z-debug.log
```

是因为在执行安装的过程中需要执行install.js，这里会下载Chromium,官网建议是进行跳过，我们可以执行 —ignore-scripts 忽略这个js执行。

```
npm i --save puppeteer --ignore-scripts
```

#### 第二种方式，设置环境变量

新建一个文件夹，打开终端窗口，进入该文件夹，设置环境变量；可阻止下载 Chromium （因为封网，直接下载会失败）

```
set PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=1  
```

再安装 puppeteer 模块

```
npm i puppeteer 
```

## 手动下载Chromium

下载地址：<a href='https://download-chromium.appspot.com/'>点击跳转</a>，打开蓝灯翻墙软件

windows系统下代码修改：

把下载刚刚下载的文件解压出来会有chrome-win32文件夹，把里面的文件拷贝到项目新建的chromium文件夹中

```
const puppeteer = require('puppeteer');
 
(async () => {
      const browser = await puppeteer.launch({
        executablePath: './chromium/chrome.exe',
        headless: false
      });
      const page = await browser.newPage();
      await page.goto('http://music.163.com/');
      await page.screenshot({path: 'music.png'});
      browser.close();
})();

```

mac系统下代码修改：

```
const puppeteer = require('puppeteer');

(async () => {
    const browser = await puppeteer.launch({executablePath: './chromium/Chromium.app/Contents/MacOS/Chromium',headless: false});
    const page = await browser.newPage();
    await page.goto('https://y.qq.com');
    await page.screenshot({path: 'yqq.png'});
    browser.close();
})();
```

## 完整示例：

```
const puppeteer = require('puppeteer');
// 豆瓣电影分类URL
const url = 'https://movie.douban.com/tag/#/?sort=U&range=5,10&tags=';

const { resolve } = require('path');

const sleep = time =>new Promise(resolve => {
    setTimeout(resolve, time);
});

(async ()=>{
    const browser = await puppeteer.launch({
        executablePath: resolve(__dirname,'./chromium/Chromium.app/Contents/MacOS/Chromium'),  //运行Chromium或Chrome可执行文件的路径
        args:['--no-sandbox'],  //传递给浏览器实例的额外参数
        dumpio:false,   //是否将浏览器的输入、输出流连通到控制台的 process.stdout和 process.stderr上。 默认为 false.
    });

    const page = await browser.newPage();
    await page.goto(url, {
        waitUntil:'networkidle2'
    });
    await sleep(3000);

    // 获取豆瓣网页上'加载更多'按钮
    await page.waitForSelector('.more');

    // 只点击一次'加载更多'
    for (let i = 0; i < 1; i++){
        await sleep(3000);
        await page.click('.more');
    }

    const result = await page.evaluate(()=>{
        // 获取豆瓣网页使用的jQuery
        var $ = window.$;
        var items = $('.list-wp a');
        var links = [];
        if (items.length >= 1){
            items.each((index, item)=>{
                let it = $(item);
                let doubanId = it.find('div').data('id');
                let title = it.find('.title').text();
                let rate = Number(it.find('.rate').text());
                let poster = it.find('img').attr('src').replace('s_ratio','l_ratio');
                links.push({
                    doubanId,
                    title,
                    rate,
                    poster
                });
            });
        }

        return links;
    });

    browser.close();
    console.log(result);

})();

```