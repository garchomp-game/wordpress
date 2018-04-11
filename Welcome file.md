<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>old_session</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__left">
    <div class="stackedit__toc">
      
<ul>
<li><a href="#sessionとoldの違いについて">sessionとoldの違いについて</a></li>
<li><a href="#sessionとoldの性質について">sessionとoldの性質について</a></li>
<li><a href="#結論：oldは必要な時だけ">結論：oldは必要な時だけ</a></li>
</ul>

    </div>
  </div>
  <div class="stackedit__right">
    <div class="stackedit__html">
      <p>session oldについて</p>
<h1 id="sessionとoldの違いについて">sessionとoldの違いについて</h1>
<p>どうも。エキセントリックエンジニアのおとかです。</p>
<p>さて、今回は、sessionとoldの違いについて紹介していくわけなんですが、なぜこれを紹介するかというと</p>
<ul>
<li>検索してもなかなかピンとくる紹介例がない</li>
<li>sessionはわかるけどoldって？て自分もなった</li>
<li>とりあえずさくっと理解できる記事あるといいよね</li>
</ul>
<p>て思ったので、今回まとめてみることにしました。</p>
<p>少しでも力になったら幸いです。</p>
<h1 id="sessionとoldの性質について">sessionとoldの性質について</h1>
<p>まずは、sessionとoldが一体どのように違うのでしょうか。以下のテーブルにまとめてみます。</p>

<table>
<thead>
<tr>
<th>session</th>
<th>old</th>
</tr>
</thead>
<tbody>
<tr>
<td>代入できる</td>
<td>代入できない</td>
</tr>
<tr>
<td>永続的に保存するようにもできるし、次のリクエストが来るまでの間のみ保存することもできる。</td>
<td>永続的に保存されない</td>
</tr>
</tbody>
</table><p>ざっくりと主要なものはこの通りになります。<br>
もともとsessionとoldは全くの別物…ではあるのですが、sessionには($request-&gt;session()-&gt;flash())があるし、結局同じフラッシュなんじゃないの？と混合した方もいると思いますが、全然そんなことはないです。</p>
<p>最も大きく違ってくるのは、例えばControllerからRequestフォルダ内にあるクラスをタイプヒントで渡している場合、はじかれた場合はoldとして取得することになります。<br>
完全に独学でLaravelをやってきたので、実際のところの正しい書き方はわかりませんが…私の場合は、</p>
<pre class=" language-php"><code class="prism :hoge.php language-php"><span class="token php language-php"><span class="token delimiter important">&lt;?php</span>
<span class="token keyword">namespace</span> <span class="token package">App<span class="token punctuation">\</span>Http<span class="token punctuation">\</span>Controllers</span><span class="token punctuation">;</span>
<span class="token keyword">use</span> <span class="token package">Illuminate<span class="token punctuation">\</span>Http<span class="token punctuation">\</span>Request</span><span class="token punctuation">;</span>
<span class="token keyword">use</span> <span class="token package">App<span class="token punctuation">\</span>Http<span class="token punctuation">\</span>Requests<span class="token punctuation">\</span>HogeRules</span><span class="token punctuation">;</span>
<span class="token keyword">class</span> <span class="token class-name">CustomerController</span> <span class="token keyword">extends</span> <span class="token class-name">Controller</span>
<span class="token punctuation">{</span>
  <span class="token keyword">public</span> <span class="token keyword">function</span> <span class="token function">index</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
  <span class="token punctuation">{</span>
    <span class="token keyword">return</span> <span class="token function">view</span><span class="token punctuation">(</span><span class="token string">'about.index'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>  
  <span class="token punctuation">}</span>

  <span class="token keyword">public</span> <span class="token keyword">function</span> <span class="token function">create</span><span class="token punctuation">(</span><span class="token punctuation">)</span>
  <span class="token punctuation">{</span>
    <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token function">old</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token variable">$array</span> <span class="token operator">=</span> <span class="token function">old</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token function">unset</span><span class="token punctuation">(</span><span class="token variable">$array</span><span class="token punctuation">[</span><span class="token string">'_token'</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token keyword">foreach</span> <span class="token punctuation">(</span><span class="token variable">$array</span> <span class="token keyword">as</span> <span class="token variable">$key</span> <span class="token operator">=</span><span class="token operator">&gt;</span> <span class="token variable">$value</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        <span class="token variable">$request</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token function">session</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token function">flash</span><span class="token punctuation">(</span><span class="token variable">$key</span><span class="token punctuation">,</span> <span class="token variable">$value</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
      <span class="token punctuation">}</span>
    <span class="token punctuation">}</span>

    <span class="token keyword">if</span> <span class="token punctuation">(</span><span class="token operator">!</span><span class="token punctuation">(</span><span class="token function">empty</span><span class="token punctuation">(</span><span class="token function">session</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token function">input</span><span class="token punctuation">(</span><span class="token string">'submit'</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      <span class="token variable">$request</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token function">session</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token function">reflash</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
    <span class="token keyword">return</span> <span class="token function">view</span><span class="token punctuation">(</span><span class="token string">'about.create'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>

  <span class="token keyword">public</span> <span class="token keyword">function</span> <span class="token function">store</span><span class="token punctuation">(</span>HogeRules <span class="token variable">$request</span><span class="token punctuation">)</span>
  <span class="token punctuation">{</span>
    <span class="token variable">$array</span> <span class="token operator">=</span> <span class="token variable">$request</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token function">all</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">unset</span><span class="token punctuation">(</span><span class="token variable">$array</span><span class="token punctuation">[</span><span class="token string">'_token'</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token function">unset</span><span class="token punctuation">(</span><span class="token variable">$array</span><span class="token punctuation">[</span><span class="token string">'submit'</span><span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    Comment<span class="token punctuation">:</span><span class="token punctuation">:</span><span class="token function">create</span><span class="token punctuation">(</span><span class="token variable">$array</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">return</span> <span class="token function">redirect</span><span class="token punctuation">(</span><span class="token string">'about/'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</span></code></pre>
<p>oldの配列の中身が空だったらfalseになるので、その部分を利用して、foreachで回してsessionに代入処理をしています。<br>
また、更新対策として、reflashも掛けるようにしています。</p>
<p>実際にstoreのHogeRulesが引っ掛かったときに帰ってくるデータはoldになるため、createでold取得し、うまく管理できるように処理を施しています。</p>
<p>このような書き方にすることで、すっきりしつつも、session oldをうまく使い分けることができました。</p>
<h1 id="結論：oldは必要な時だけ">結論：oldは必要な時だけ</h1>
<p>基本的に永続的、または一時的限らず、sessionを使うようにしましょう。<br>
oldは受け取る際にどうしても必要な時は使うようにし、複雑でなければビューでそのままoldしてもいいのですが、複雑なケースが発生する可能性がある場合は、あらかじめoldをガッツリコントローラーで取得して処理を挟むと拡張しやすいかもしれませんね。</p>

    </div>
  </div>
</body>

</html>
