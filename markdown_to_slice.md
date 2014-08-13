# markdown to slice
这几天在准备写西伯利亚之行的游记，还想做成幻灯片向好友展示，因为之前都用markdown写游记并发到博客上，所以研究了几款markdown to slice工具

## reveal.js
- [官网github.com/hakimel/reveal.js](https://github.com/hakimel/reveal.js)
- 功能强大的slice制作工具
- md只是一部分，还有动画，主题，表格，js自定义，背景色，背景图，代码高亮，md文件分离，离线演示，
- [教程](http://lab.hakim.se/reveal-js/)
- [配套的适合非程序员的在线编辑网站](http://slides.com)
- 优点
    - 幻灯片演示功能强大，ESC全局预览，上下左右浏览，支持动画
    - 可以用github page
- 缺点
    - 语法稍微复杂，比如插入图片需要专门的语法，基本md玩不转

## remark
- [官网github.com/gnab/remark](https://github.com/gnab/remark)
- 轻量级md->slice工具
- 优点
    - 对原生md语法支持很好
    - 对
    - 支持代码高亮
    - 支持github page

## swipe
- [官网www.swipe.to](https://www.swipe.to/)
- 这其实是个在线制作/演示幻灯片的网站，提交markdown格式文档并转换成在线的幻灯网页其实是它的副产品
- [官方教程](https://www.swipe.to/markdown/)
- 优点
    - 正如网站所说："You write, we do the rest" 可以用原生md语法写作，只要记住***是换页就可以了，高效，快速！
    - 支持html语言的扩展
    - 支持背景图片，文字颜色和字体的自定义
    - 提供演示网页，不需要自己建站，演示网页带个访问统计
- 缺点
    - 用md语法插入的图片宽度受限，大概800-1000px左右，而用网站直接插入的图片可以全屏浏览



## 总结
- 混合使用reveal.js/remark
- md书写用，直接commit到github
    - 换行使用***和****
- 演示用，准备写个在线小程序转换格式
- reveal    - 
    - 使用换行符(而不是---或者;;;等)切换幻灯片，保护原生md文件格式 `<section data-markdown="example.md" data-separator="^\n\n\n" data-vertical="^\n\n"></section>`
- reveal 初始化过程
    - download newest reveal.js from [https://github.com/hakimel/reveal.js/releases](https://github.com/hakimel/reveal.js/releases) 
    - edit template
        - add externel markdown
    - add google analysis id to index.html
        - new google analysis id
        - add js code before `</body>` of html page
- reveal 使用过程
    - copy template
    - update `<title>马年上半年我去过哪些地方</title>`
    - add md file and update the file name in `<section data-markdown="example.md" data-separator="^\n\n\n" data-vertical="^\n\n"></section>`