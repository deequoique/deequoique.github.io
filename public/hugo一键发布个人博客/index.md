# Hugo一键发布个人博客

 以vscode为例，在项目根文件夹建立.vscode文件夹，文件夹内创建`tasks.json`文件，内容如下
&lt;!--more--&gt;
``` json
{
    &#34;label&#34;: &#34;Publish Blog&#34;,
    &#34;type&#34;: &#34;shell&#34;,
    &#34;command&#34;: &#34;hugo --theme=你的主题 --baseURL=\&#34;你的站点\&#34; &amp;&amp; cd public &amp;&amp; git add . &amp;&amp; git commit -m \&#34;Commit message\&#34; &amp;&amp; git push &amp;&amp; cd ..&#34;,
    &#34;group&#34;: {
        &#34;kind&#34;: &#34;build&#34;,
        &#34;isDefault&#34;: true
    },
    &#34;presentation&#34;: {
        &#34;reveal&#34;: &#34;always&#34;,
        &#34;panel&#34;: &#34;shared&#34;,
        &#34;showReuseMessage&#34;: false,
        &#34;clear&#34;: false
    },
    &#34;problemMatcher&#34;: []
}
```
### 配置解释

- **label**: 任务名称。
- **type**: 任务类型，这里是 &#34;shell&#34;，意味着它将在 shell 环境下执行。
- **command**: 要执行的命令序列。
    - `hugo --theme=Loveit --baseURL=&#34;https://onlane.github.io/&#34;`：运行 Hugo 命令来生成站点，使用 Loveit 主题，设置基础 URL 为你的远端仓库地址。
    - `cd public`：改变目录到 `public`。
    - `git add . &amp;&amp; git commit -m &#34;Commit message&#34; &amp;&amp; git push`：将更改添加到 Git 暂存区，提交这些更改，并推送到远程仓库。
    - `cd ..`：返回到先前的目录。
- **group**: 将此任务分组到 &#34;build&#34; 类别。
- **presentation**: 控制任务运行时的终端展示方式。
    - **reveal**: 设置为 &#34;always&#34;，在任务运行时总是显示终端。
    - **panel**: 使用共享的面板。
- **problemMatcher**: 用于匹配输出中的问题，这里设置为空。

Shift&#43;ctrl&#43;P运行命令，选择该任务即可。

---

> Author: Deequoique  
> URL: http://localhost:1313/hugo%E4%B8%80%E9%94%AE%E5%8F%91%E5%B8%83%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/  

