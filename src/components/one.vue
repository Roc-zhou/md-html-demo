<template>
  <div class="hello">
    <div class="markdown-body">
      <h1><a id="1_0"></a>1</h1>
    <p><strong>粗体</strong><br />
    <em>斜体</em></p>
    <h3><a id="_4"></a>三级标题</h3>
    <p><ins>下划线</ins><br />
    <s>中划线</s><br />
    <mark>标记</mark></p>
    <div class="hljs-left">
    <p>居左</p>
    </div>
    <div class="hljs-center">
      <p>居中</p>
    </div>
    <div class="hljs-right">
    <p>居右</p>
    </div>
    <blockquote>
    <p>段落引用</p>
    </blockquote>
    <p><a href="www.baidu.com" target="_blank">百度</a></p>
    <pre><div class="hljs"><code class="lang-python">    <span class="hljs-keyword">if</span> <span class="hljs-string">'ActualNickName'</span> <span class="hljs-keyword">in</span> msg:
            <span class="hljs-comment"># 昵称</span>
            from_user = msg[<span class="hljs-string">'ActualNickName'</span>]
            print(<span class="hljs-string">'%s : %s'</span> % (from_user, msg[<span class="hljs-string">'Text'</span>]))
        <span class="hljs-keyword">elif</span> msg[<span class="hljs-string">'FromUserName'</span>] == <span class="hljs-string">'@34d4ce1506afbc8337aa692dba2689d9f5cd57ed57165acb003babccca086bbd'</span> :
            print(<span class="hljs-string">'%s : %s'</span> % (<span class="hljs-string">'我说：'</span>, msg[<span class="hljs-string">'Text'</span>]))
        <span class="hljs-keyword">else</span>:
            defaultMsg = <span class="hljs-string">'hey'</span>
            okMsg = get_res(msg[<span class="hljs-string">'Text'</span>])
            print(<span class="hljs-string">'消息 : %s'</span> % (msg[<span class="hljs-string">'Text'</span>]))
            print(okMsg <span class="hljs-keyword">or</span> defaultMsg)
            msg.user.send(okMsg <span class="hljs-keyword">or</span> defaultMsg)
    </code></div></pre>
    <table>
    <thead>
    <tr>
    <th>column1</th>
    <th>column2</th>
    <th>column3</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td>content1</td>
    <td>content2</td>
    <td>content3</td>
    </tr>
    </tbody>
    </table>
    </div>
  </div>
</template>

<script>
import 'mavon-editor/dist/css/index.css'
import '../assets/github-markdown.min.css'
import '../assets/github.min.css'
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      value:''
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
