<template>
  <div id="app">
    <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
    <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
  </div>
</template>

<script>
  import StyleEditor from './components/StyleEditor'
  import ResumeEditor from './components/ResumeEditor'
  import './assets/reset.css'

  export default {
    name: 'app',
    components: {
      StyleEditor,
      ResumeEditor
    },
    data() {
      return {
        interval: 10,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [
          `/*
* Hello everyone，我是chenfx —— 一个"学渣"
* 看了大神的项目，受到启发，想自己也来整一波，顺便膜拜一下大神
* 下面即将开始：
* let's go………………

*/

/* 来一波过渡效果 */
* {
  -webkit-transition: all .3s;
  transition: all .3s;
}
/* 千篇一律的白色，我们来点儿其它背景 */
html {
  color: #f60;
  background: #505050;
}
/* 文字离边框太近了 */
.styleEditor {
  padding: .5em;
  border: 1px solid #f60;
  margin: .5em;
  overflow: auto;
  overflow-x: hidden;
  width: 45vw;
  height: 96vh;
}


/* 3D 效果更炫酷 */
html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
/* 位置挪一挪 各位换个坐 */
.styleEditor {
  position: fixed;
  left: 0;
  top: 0;
  transform: translateX(110%) rotateY(-370deg) translateZ(-100px);
}

/* 接下来一个勉强可用的编辑器，给自己一点儿"shen力" */
.resumeEditor{
  position: fixed;
  right: 0;
  top: 0;
  -webkit-transition: all 3s;
  transition: all 3s;
  transform: rotateY(360deg) translateX(-100%) rotateY(10deg) translateZ(-100px);
  padding: .5em;
  margin: .5em;
  width: 48vw;
  height: 96vh;
  border: 1px solid #f60;
  background: #505050;
  color: #f60;
  overflow: auto;
}
/* 没什么写的，只能拿自己的简历练练手 */


`,
          `
/* 这个简历好像差点什么
 * 对了，这是 Markdown 格式的，我需要变成对 HR 更友好的格式
 * 简单，用开源工具翻译成 HTML 就行了
 */
`
          ,
          `
/* 再对 HTML 加点样式 */
.resumeEditor{
  background:#505050;
  padding: 2em;
  height: 96vh;
  color: #f60;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ul li a{
  color: #f60;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;
  content: counters(section, ".") " ";
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: #ddd;
}


/*
**◢████████████◣　　　　　　　
**　　　██████████████　　　　　　　
**　　　██　　　◥██◤　　　██　　　　　　　
**　◢███　　　　◥◤　　　　██◣　　　　　　
**　▊▎██◣　　　　　　　　◢█▊▊　　　　　　
**　▊▎██◤　　●　　●　　◥█▊▊　　　　　
**　▊　██　　　　　　　　　　█▊▊　　　　　　
**　◥▇██　▊　　　　　　▊　█▇◤　　　　　　
**　　　██　◥▆▄▄▄▄▆◤　█▊　　　◢▇▇◣
**◢██◥◥▆▅▄▂▂▂▂▄▅▆███◣　▊◢　█
**█╳　　　　　　　　　　　　　　　╳█　◥◤◢◤
**◥█◣　　　˙　　　　　˙　　　◢█◤　　◢◤　
**　　▊　　　　　　　　　　　　　▊　　　　█　　
**　　▊　　　　　　　　　　　　　▊　　　◢◤　　
**　　▊　　　　　　⊕　　　　　　█▇▇▇◤　　
**　◢█▇▆▆▆▅▅▅▅▆▆▆▇█◣　　　　　　
**　▊　▂　▊　　　　　　▊　▂　
**
*/



基于strml.net灵感
copyright©chenfx
`],
        currentMarkdown: '',
        fullMarkdown: `陈发祥
----
* 毕业院校：重庆师范大学
* 专业：计算机科学与技术
* 年龄：XXX
* 技术方向：前端工程师、.NET开发、嵌入式开发
* 爱好: 爬山、篮球
* 目标：小小的天大大的梦想……


技能
----

* HTML 开发
* Node.js 开发
* .NET开发
* 嵌入式开发——哈哈哈

工作经历
----

### 2015.07 —— 2015.09 重庆启高科技有限公司前端实习

* 参与公司平台的页面开发

### 2015.09 - 至今 重庆猪八戒网络有限公司 - 前端工程师

* 正式从事前端开发工作

友情链接
----

* [blog](http://www.chenfx.com.cn/)
* [GitHub](https://github.com/chenfaxiang)


`
      }
    },
    created() {
      this.makeResume()
    },

    methods: {
      makeResume: async function () {
        await this.progressivelyShowStyle(0)
        await this.progressivelyShowResume()
        await this.progressivelyShowStyle(1)
        await this.showHtml()
        await this.progressivelyShowStyle(2)
      },
      showHtml: function () {
        return new Promise((resolve, reject) => {
          this.enableHtml = true
          resolve()
        })
      },
      progressivelyShowStyle(n) {
        return new Promise((resolve, reject) => {
          let interval = this.interval
          let showStyle = (async function () {
            let style = this.fullStyle[n]
            if (!style) { return }
            // 计算前 n 个 style 的字符总数
            let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
            let prefixLength = length - style.length
            if (this.currentStyle.length < length) {
              let l = this.currentStyle.length - prefixLength
              let char = style.substring(l, l + 1) || ' '
              this.currentStyle += char
              if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                this.$nextTick(() => {
                  this.$refs.styleEditor.goBottom()
                })
              }
              setTimeout(showStyle, interval)
            } else {
              resolve()
            }
          }).bind(this)
          showStyle()
        })
      },
      progressivelyShowResume() {
        return new Promise((resolve, reject) => {
          let length = this.fullMarkdown.length
          let interval = this.interval
          let showResume = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              console.log(prevChar)
              if (prevChar === '\n' && this.$refs.resumeEditor) {
                this.$nextTick(() => this.$refs.resumeEditor.goBottom())
              }
              setTimeout(showResume, interval)
            } else {
              resolve()
            }
          }
          showResume()
        })
      }
    }
  }

</script>

<style scoped>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  html {
    min-height: 100vh;
  }

  * {
    -webkit-transition: all .3s;
    transition: all .3s;
  }
</style>
