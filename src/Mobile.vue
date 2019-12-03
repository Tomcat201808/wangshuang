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
        interval: 5,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [
          `/*
* Inspired by    缘分~
* 
*
* 霜儿，我是福君————
* 一个很喜欢你，但是又不知如何表达的呆子
* 作为程序员，我想写一份自我介绍，以表达爱慕之情
* 方便你了解更真实的我
*
*
*/

/* 给所有元素加上过渡效果 */
* {
  transition: all .2s;
}
/* 设置背景颜色 */
html {
  color: rgb(222,222,222);
  background: rgb(0,43,54);
}
/* 设置边框 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  overflow: auto;
  width: 90vw;
  margin: 2.5vh 5vw;
  height: 90vh;
}
/* 太高了 */
.styleEditor {
  height: 45vh;
}
/* 代码高亮 */
.token.selector{
  color: rgb(133,153,0);
}
.token.property{
  color: rgb(187,137,0);
}
.token.punctuation{
  color: yellow;
}
.token.function{
  color: rgb(42,161,152);
}

/* 加3D效果 */
html{
  perspective: 1000px;
}
.styleEditor {
  position: fixed; left: 0; top: 0;
  transform: rotateX(-10deg) translateZ(-50px) ;
}

/* 准备一个编辑器 */
.resumeEditor{
  position: fixed;
  top: 50%; left: 0;
  padding: .5em;  margin: 2.5vh;
  width: 95vw; height: 45vh;
  border: 1px solid;
  background: white; color: #222;
  overflow: auto;
}
/* 开始写咯..... */


`,
          `
/*将Markdown格式翻译成HTML
 *再对HTML加点样式
*/
`, `
.resumeEditor{
  padding: 2em;
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
`],
        currentMarkdown: '',
        fullMarkdown: `福君
======================================
坐标：深圳福田。

Java架构师，大数据项目经理，机器学习算法工程师

技能
====

数据库设计
----
  - 结构化数据库设计
  - 数据仓库模型设计

后端开发
----
  - 用户管理
  - 多点登录，消息漫游
  - 交易系统
  - 支付系统
  - 公众号开发

前端开发
----
  - Web前端开发

产品设计
----
  - 省级银行大数据平台
  - 某县城电子政务平台
  - 天眼监控平台
  - CMCC-BOSS(中国移动运营支撑管理系统)

技术及语言
----
  - Java:      JVM, Springboot, MyBatis
  - Big Data:  Hadoop, Zookeeper, Hive, Spark, Storm, Flink，Apache Beam
  - Python:    pandas, numpy, matplotlib,sklearn
  - DataBase:  Oracle, MySQL, GreenPlum, Hbase, Redis
  - WebServer: Tomcat, nginx
  - OS:        CentOS, UNIX, Windows
  - Others:    git, Xmind

工作经历
----

1. 中国移动通信集团广西有限公司
2. 中国银行深圳分行


教育经历
----

1. 广西大学 计算机科学与技术 学士
....(学无止境...)

文章
====

* [读《亲密关系》 ](https://mp.weixin.qq.com/s/PIlYV_v0ZI2L6H2EQv2anw)
* [寻找那些长期对你好的人](https://mp.weixin.qq.com/s/6BFAY1p1U4Is7cTopyhdfg)
* [数据库老头儿的独孤九剑](https://mp.weixin.qq.com/s/LVWEAcw1rlT2BfhDZ-qDBQ)
* [太史公的智慧——也谈谈《货殖列传》](https://mp.weixin.qq.com/s/HYrTyjTXp3oOs-_sJm8ytw)
* [深度学习——卷积神经网络的奇妙](https://mp.weixin.qq.com/s/lPrM-HelOBrb0MwlJ65NOw)
* [关于三个概况人类文明公式的感悟](https://mp.weixin.qq.com/s/4okM9FeGKE5_IZ8lM4OJFg)
* [精神资源与深度关系](https://mp.weixin.qq.com/s/mtVVuJU9Pqel4ehfuv__qQ)

链接
====

* 
* [微信公众号](概率邦)
* 


勾引方式
====

* 微信：weifujun2

感恩
----

* 感谢命运，感谢缘分，感恩你我相遇相知。你于我，独一无二，无人可取代————
* 1.有幸看到你的执着，仿佛看到了我自己，我们早已是很好的朋友；
* 2.早已认你是个知己，因为你对人生的不卑不亢、不屈不挠；
* 3.人生苦短，我们应该结伴而行，我未娶你未嫁，我们应该开启一段亲密关系之旅；
* 4.我们都能成为更好的自己,彼此适配协调,明天会更好,霜儿是否愿意携手同行？
* ...
* 未完待续...



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
          this.$nextTick(() => {
            this.$refs.resumeEditor.goTop()
          })
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
    min-height: 100vh; position: relative;
  }

  html {
    min-height: 100vh;
  }
  *{
    box-sizing: border-box;
  }

</style>
