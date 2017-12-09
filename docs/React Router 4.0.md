
#### reactRouter4.0

RR4 本次采用单代码仓库模型架构（monorepo），这意味者这个仓库里面有若干相互独立的包，分别是：

- react-router React Router 核心
- react-router-dom 用于 DOM 绑定的 React Router
- react-router-native 用于 React Native 的 React Router
- react-router-redux React Router 和 Redux 的集成
- react-router-config 静态路由配置的小助手

#### 引用

**react-router 还是 react-router-dom？**

在 React 的使用中，我们一般要引入两个包，react 和 react-dom，那么 react-router 和  react-router-dom 是不是两个都要引用呢？


非也，坑就在这里。他们两个只要引用一个就行了，不同之处就是后者比前者多出了 <Link> <BrowserRouter> 这样的 DOM 类组件。


因此我们只需引用 react-router-dom 这个包就行了。当然，如果搭配 redux ，你还需要使用 react-router-redux。


#### 组件

**1. <BrowserRouter>**

此组件拥有以下属性：

==basename: string==

作用：为所有位置添加一个基准URL
使用场景：假如你需要把页面部署到服务器的二级目录，你可以使用 basename 设置到此目录。


```js
<BrowserRouter basename="/minooo" />
<Link to="/react" /> // 最终渲染为 <a href="/minooo/react">
```


==getUserConfirmation: func==

作用：导航到此页面前执行的函数，默认使用 window.confirm
使用场景：当需要用户进入页面前执行什么操作时可用，不过一般用到的不多。


```js
const getConfirmation = (message, callback) => {
  const allowTransition = window.confirm(message)
  callback(allowTransition)
}

<BrowserRouter getUserConfirmation={getConfirmation('Are you sure?', yourCallBack)} />
```

==forceRefresh: bool==

作用：当浏览器不支持 HTML5 的 history API 时强制刷新页面。
使用场景：同上。


```js
const supportsHistory = 'pushState' in window.history
<BrowserRouter forceRefresh={!supportsHistory} />
```


==keyLength: number==

作用：设置它里面路由的 location.key 的长度。默认是6。（key的作用：点击同一个链接时，每次该路由下的 location.key都会改变，可以通过 key 的变化来刷新页面。）
使用场景：按需设置。

    <BrowserRouter keyLength={12} />
    
==children: node==

作用：渲染唯一子元素。
使用场景：作为一个 Reac t组件，天生自带 children 属性。

**2. <Route>**

<Route> 自带三个 render method 和三个 props 。

==render methods 分别是：==

- <Route component>
- <Route render>
- <Route children>

每种 render method 都有不同的应用场景，同一个<Route> 应该只使用一种 render method ，大部分情况下你将使用 component 。

==props 分别是：==

- match
- location
- history

所有的 render method 无一例外都将被传入这些 props。

**3. <Link>**

==to: string==

作用：跳转到指定路径
使用场景：如果只是单纯的跳转就直接用字符串形式的路径。

    <Link to="/courses" />
    
==to: object==

作用：携带参数跳转到指定路径
作用场景：比如你点击的这个链接将要跳转的页面需要展示此链接对应的内容，又比如这是个支付跳转，需要把商品的价格等信息传递过去。

    <Link to={{
      pathname: '/course',
      search: '?sort=name',
      state: { price: 18 }
    }} />
    
==replace: bool==

为 true 时，点击链接后将使用新地址替换掉上一次访问的地址，什么意思呢，比如：你依次访问 '/one' '/two' '/three' ’/four' 这四个地址，如果回退，将依次回退至 '/three' '/two' '/one' ，这符合我们的预期，假如我们把链接 '/three' 中的 replace 设为 true 时。依次点击 one two three four 然后再回退会发生什么呢？会依次退至 '/three' '/one'！



**4. <NavLink>**

这是 <Link> 的特殊版，顾名思义这就是为页面导航准备的。因为导航需要有 “激活状态”。

==activeClassName: string==

导航选中激活时候应用的样式名，默认样式名为 active

    <NavLink
      to="/about"
      activeClassName="selected"
    >MyBlog</NavLink>
    
==activeStyle: object==

如果不想使用样式名就直接写style

    <NavLink
      to="/about"
      activeStyle={{ color: 'green', fontWeight: 'bold' }}
    >MyBlog</NavLink>
    
==exact: bool==

若为 true，只有当访问地址严格匹配时激活样式才会应用

==strict: bool==

若为 true，只有当访问地址后缀斜杠严格匹配（有或无）时激活样式才会应用

==isActive: func==

决定导航是否激活，或者在导航激活时候做点别的事情。不管怎样，它不能决定对应页面是否可以渲染。

**5 .<Switch>**

只渲染出第一个与当前访问地址匹配的 <Route> 或 <Redirect>。

用途：
- 将多个 <Route> 组合到应用程序中，例如侧边栏（sidebars），面包屑 等等。
- <Switch> 对于转场动画也非常适用，因为被渲染的路由和前一个被渲染的路由处于同一个节点位置！
- 
==children: node==

<Switch> 下的子节点只能是 <Route> 或 <Redirect> 元素。只有与当前访问地址匹配的第一个子节点才会被渲染。<Route> 元素用它们的 path 属性匹配，<Redirect> 元素使用它们的 from 属性匹配。如果没有对应的 path 或 from，那么它们将匹配任何当前访问地址。

**6. <Redirect>**

<Redirect>渲染时将导航到一个新地址，这个新地址覆盖在访问历史信息里面的本该访问的那个地址。

==to: string==

重定向的 URL 字符串

==to: object==

重定向的 location 对象

==push: bool==

若为真，重定向操作将会把新地址加入到访问历史记录里面，并且无法回退到前面的页面。

==from: string==

需要匹配的将要被重定向路径。


**7. Prompt**

当用户离开当前页面前做出一些提示。

==message: string==

当用户离开当前页面时，设置的提示信息。

    <Prompt message="确定要离开？" />

==message: func==

当用户离开当前页面时，设置的回掉函数

    <Prompt message={location => (
      `Are you sue you want to go to ${location.pathname}?` 
    )} />
    
==when: bool==

通过设置一定条件要决定是否启用 Prompt


#### 对象和方法

**1. history**

histoty 是 RR4 的两大重要依赖之一（另一个当然是 React 了），在不同的 javascript 环境中， history 以多种能够行驶实现了对会话（session）历史的管理。

我们会经常使用以下术语：

- "browser history" - history 在 DOM 上的实现，用于支持 HTML5 history API 的浏览器
- "hash history" - history 在 DOM 上的实现，用于旧版浏览器。
- "memory history" - history 在内存上的实现，用于测试或非 DOM 环境（例如 React Native）。


history 对象通常具有以下属性和方法：

- length: number  浏览历史堆栈中的条目数
- action: string 路由跳转到当前页面执行的动作，分为 PUSH, REPLACE, POP
- location: object 当前访问地址信息组成的对象，具有如下属性：
    + pathname: string URL路径
    + search: string URL中的查询字符串
    + hash: string URL的 hash 片段
    + state: string 例如执行 push(path, state) 操作时，location 的 state 将被提供到堆栈信息里，state 只有在 browser 和 memory history 有效。
- push(path, [state]) 在历史堆栈信息里加入一个新条目。
- replace(path, [state]) 在历史堆栈信息里替换掉当前的条目
- go(n) 将 history 堆栈中的指针向前移动 n。
- goBack() 等同于 go(-1)
- goForward 等同于 go(1)
- block(prompt) 阻止跳转

history 对象是可变的，因此建议从 <Route> 的 prop 里来获取 location，而不是从 history.location 直接获取。这样可以保证 React 在生命周期中的钩子函数正常执行，例如以下代码：

    class Comp extends React.Component {
      componentWillReceiveProps(nextProps) {
        // locationChanged
        const locationChanged = nextProps.location !== this.props.location

        // 错误方式，locationChanged 永远为 false，因为history 是可变的
        const locationChanged = nextProps.history.location !== this.props.history.location
      }
    }

<h4>2. location</h4>
<p>location 是指你当前的位置，将要去的位置，或是之前所在的位置</p>
<pre><code class="javascript">{
  key: 'sdfad1'
  pathname: '/about',
  search: '?name=minooo'
  hash: '#sdfas',
  state: {
    price: 123
  }
}</code></pre>
<p>在以下情境中可以获取 location 对象</p>
<ul>
<li>在 <code>Route component</code> 中，以 this.props.location 获取</li>
<li>在 <code>Route render</code> 中，以 ({location}) =&gt; () 方式获取</li>
<li>在 <code>Route children</code> 中，以 ({location}) =&gt; () 方式获取</li>
<li>在 <code>withRouter</code> 中，以 this.props.location 的方式获取</li>
</ul>
<p>location 对象不会发生改变，因此可以在生命周期的回调函数中使用 location 对象来查看当前页面的访问地址是否发生改变。这种技巧在获取远程数据以及使用动画时非常有用</p>
<pre><code class="javascript">componentWillReceiveProps(nextProps) {
  if (nextProps.location !== this.props.location) {
    // 已经跳转了！
  }
}</code></pre>
<p>可以在不同情境中使用 location：</p>
<ul>
<li><code>&lt;Link to={location} /&gt;</code></li>
<li><code>&lt;NaviveLink to={location} /&gt;</code></li>
<li><code>&lt;Redirect to={location /&gt;</code></li>
<li>history.push(location)</li>
<li>history.replace(location)</li>
</ul>

<h4>3. match</h4>
<p>match 对象包含了 &lt;Route path&gt; 如何与 URL 匹配的信息，具有以下属性：</p>
<ul>
<li>params: object 路径参数，通过解析 URL 中的动态部分获得键值对</li>
<li>isExact: bool 为 true 时，整个 URL 都需要匹配</li>
<li>path: string 用来匹配的路径模式，用于创建嵌套的 &lt;Route&gt;</li>
<li>url: string URL 匹配的部分，用于嵌套的 &lt;Link&gt;</li>
</ul>
<p>在以下情境中可以获取 match 对象</p>
<ul>
<li>在 <code>Route component</code> 中，以 this.props.match获取</li>
<li>在 <code>Route render</code> 中，以 ({match}) =&gt; () 方式获取</li>
<li>在 <code>Route children</code> 中，以 ({match}) =&gt; () 方式获取</li>
<li>在 <code>withRouter</code> 中，以 this.props.match的方式获取</li>
<li>matchPath 的返回值</li>
</ul>
<p>当一个 Route 没有 path 时，它会匹配一切路径。</p>


**参考链接**

 [React Router中文文档](https://react-guide.github.io/react-router-cn/)
 
[ 初探 React Router 4.0](http://www.jianshu.com/p/e3adc9b5f75c)




