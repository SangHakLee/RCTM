---


---

<h1 id="javascript-null-check-빈-값-체크-v2019"># [JavaScript] Null check, 빈 값 체크 v2019</h1>
<h3 id="introduction">

### Introduction</h3>
<p>
필자의 블로그 중 가장 조회수가 높은 글이다.</p>
<p>

세번째 게시글인 만큼 블로그 초장기에 작성한 내용이다.</p>
<p>

근 일년간 블로그 활동에 소홀했었다. 다시 개발 블로그를 열심히 하기로 마음을 먹고 블로그 통계를 확인했다.<br>
어떤 게시글들이 인기 있는지 확인해보고 싶어서였다.</p>
<h2 id="isempty">isEmpty</h2>
<p><code>gist</code>에 공유한 <a href="


## isEmpty
`gist`에 공유한 [isEmpty](https://gist.github.com/SangHakLee/4da6159a7a08cdd12132">isEmpty</a>는 문제점이 있다.  <code>false</code>)는 문제점이 있다.  `false` 입력 값에 대해선 의도와 다른 동작을 한다.</p>
<p>

그래서 최초에 의도한 기능대로 동작하는 <code>`isEmpty</code>`를 다시 작성했다.</p>
<p><code>is-</code>

`is-`로 시작하는 함수 관리하기 위해 <code>is</code>`is`라는 NPM 모듈로 만들었다.</p>
<h3 id="is">is</h3>
<p><a href="

### is
[https://sanghaklee.github.io/is">](https://sanghaklee.github.io/is</a></p>
<pre class=" language-javascript"><code class="prism  language-javascript">is<span class="token punctuation">.</span><span class="token function">empty</span><span class="token punctuation">(</span><span class="token string">''</span><span class="token punctuation">)</span> <span class="token comment">// true</span>
</code></pre>
<h3 id="빈-값">빈 값</h3>
<p><code>isEmpty</code>는 파라미터 값이 비었는지 확인한다. <strong>빈 값</strong>이라 정의한 값들은 아래와 같다.</p>
<ul>
<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String">String</a>
<ul>
<li><code>''</code></li>
<li><code>new String()</code></li>
<li><strong>``</strong> <a href=")
```javascript
is.empty('') // true
```

### 빈 값
`isEmpty`는 파라미터 값이 비었는지 확인한다. **빈 값**이라 정의한 값들은 아래와 같다.


- [String](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String)
	- `''`
	- `new String()`
	- **``** [Template literals
](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals">)
	- `${''}` [Template literals<br>
</a></li>
<li><code>${''}</code> <a href="
](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Template_literals">Template literals<br>
</a></li>
</ul>
</li>
<li><a href=")
- [Array](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array">Array</a>
<ul>
<li><code>[]</code></li>
<li><code>new Array()</code></li>
</ul>
</li>
<li><a href="https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object">Object</a>
<ul>
<li><code>{}</code></li>
<li><code>new Object()</code></li>
<li><code>new Proxy({}, {})</code> <a href=")
	- `[]`
	- `new Array()`
- [Object](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object)
	- `{}`
	- `new Object()`
	- `new Proxy({}, {})` [Proxy](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy">Proxy</a></li>
</ul>
</li>
<li><a href=")
- [null](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/null">null</a>
<ul>
<li><code>null</code></li>
</ul>
</li>
<li><a href=")
	- `null`
- [undefined](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/undefined">undefined</a>
<ul>
<li><code>undefined</code></li>
</ul>
</li>
</ul>
<h3 id="채워진-값">채워진 값</h3>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token number">1</span>
<span class="token string">'string'</span>
<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span>
<span class="token punctuation">{</span>a<span class="token punctuation">:</span><span class="token number">1</span><span class="token punctuation">}</span>
</code></pre>
<ul>
<li>의심의 여지가 없는 명백하게 채워진 값들이다.</li>
</ul>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token boolean">true</span>
<span class="token boolean">false</span>
</code></pre>
<ul>
<li><code>boolean</code>도 값이다. 특히 <code>false</code>도 값이다.</li>
</ul>
<h4 id="object">Object</h4>
<p><code>Object</code>는 까다롭다.</p>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">new</span> <span class="token class-name">Boolean</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">// Boolean{false}</span>
<span class="token boolean">false</span> <span class="token comment">// false</span>

<span class="token keyword">typeof</span> <span class="token keyword">new</span> <span class="token class-name">Boolean</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">// object</span>
<span class="token keyword">typeof</span> <span class="token boolean">false</span> <span class="token comment">// boolean</span>

<span class="token keyword">typeof</span> <span class="token keyword">new</span> <span class="token class-name">Boolean</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">!=</span> <span class="token keyword">typeof</span> <span class="token boolean">false</span>
<span class="token keyword">new</span> <span class="token class-name">Boolean</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">==</span> <span class="token boolean">false</span>
</code></pre>
<p><a href="https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip">https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip</a></p>
<pre class=" language-javascript"><code class="prism  language-javascript"><span class="token keyword">new</span> <span class="token class-name">Boolean</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">// Boolean{false}, == false</span>
<span class="token keyword">new</span> <span class="token class-name">Number</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">// Number{0}, == 0</span>
<span class="token keyword">new</span> <span class="token class-name">Date</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">// Thu Jun 27 2019 00:59:56 GMT+0900 (한국 표준시)</span>
<span class="token keyword">new</span> <span class="token class-name">RegExp</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">// /(?:)/</span>
<span class="token keyword">new</span> <span class="token class-name">Error</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token comment">)
	- `undefined`


### 채워진 값
```javascript
1
'string'
[1]
{a:1}
```
- 의심의 여지가 없는 명백하게 채워진 값들이다. 


```javascript
true
false
```
- `boolean`도 값이다. 특히 `false`도 값이다.

#### Object
`Object`는 까다롭다. 
```javascript
new Boolean() // Boolean{false}
false // false

typeof new Boolean() // object
typeof false // boolean

typeof new Boolean() != typeof false
new Boolean() == false
```
https://stackoverflow.com/questions/17256182/what-is-the-difference-between-string-primitives-and-string-objects-in-javascrip


```javascript
new Boolean() // Boolean{false}, == false
new Number() // Number{0}, == 0
new Date() // Thu Jun 27 2019 00:59:56 GMT+0900 (한국 표준시)
new RegExp() // /(?:)/
new Error() // Error at &lt;<anonymous&gt;</span>
</code></pre>
<ul>
<li>위 객체들은 자명하게 값을 리턴</li>
</ul>
<hr>
<p><a href="https://github.com/SangHakLee/is/blob/master/test/datas/empty.js">Test Data-set</a>을 보면 필자가 어떤 값들을 <strong>빈 값</strong>이라 보고 어떤 값들을 <strong>채워진 값</strong>이라 보는지 알 수 있다.</p>
<h2 id="u>
```
- 위 객체들은 자명하게 값을 리턴

---


[Test Data-set](https://github.com/SangHakLee/is/blob/master/test/datas/empty.js)을 보면 필자가 어떤 값들을 **빈 값**이라 보고 어떤 값들을 **채워진 값**이라 보는지 알 수 있다.

## Underscore.js--l & Lodash">
[Underscore.js &amp; Lodash</h2>
<p><a href="https://underscorejs.org">Underscore.js</a>. <a href="https://lodash.com">Lodash</a>에 <code>isEmpty</code>가 이미 존재한다.</p>
<p><a href="](https://underscorejs.org). [Lodash](https://lodash.com)에 `isEmpty`가 이미 존재한다.

[map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map">map</a> 을 두고 <a href=") 을 두고 [_.map](https://underscorejs.org/#map"><em>.map</em></a>) 을 쓸 정도로 Undederscore.js 성애자인 필자가 <a href="[_.isEmpty](https://underscorejs.org/#isEmpty">.isEmpty</a>) 를 사용하지 않는 이유가 있다.</p>
<p>

필자가 원하는 것은 <strong>**파라미터의 값이 비었는지 확인</strong>**하는 것이다. 하지만, <code>`_.isEmpty</code>`는 필자의 의도와 다르게 동작한다.</p>
<ul>
<li><code>boolean</code> return <strong>true</strong>
<ul>
<li>_.isEmpty(true)</li>
</ul>
</li>
<li><code>number</code> return <strong>true</strong>
<ul>
<li>_.isEmpty(1)</li>
</ul>
</li>
<li><code>object created with new, except new Object()</code> <strong>true</strong>
<ul>
<li>_.isEmpty(new Date())</li>
</ul>
</li>
<li><code>

- `boolean` return **true**
	- _.isEmpty(true)
- `number` return **true**
	- _.isEmpty(1)
- `object created with new, except new Object()` **true**
	- _.isEmpty(new Date())
- `function</code>` return <strong>true</strong>
<ul>
<li>_.isEmpty(function() {})</li>
</ul>
</li>
</ul>
<p>필자는 앞에서 <strong>빈 값</strong>**true**
	- _.isEmpty(function() {})

필자는 앞에서 **빈 값**이라  정의한 파라미터를 제외한 모든 경우를 <strong>채워진 값</strong>**채워진 값**으로 판단하고 싶었다.<br>
JavaScript로 데이터를 다루다 보면 이렇게 처리해야 하는 경우가 꽤 생긴다.</p>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY2MjU5NzAyNF19
-->