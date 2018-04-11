<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Welcome file</title>
  <link rel="stylesheet" href="https://stackedit.io/style.css" />
</head>

<body class="stackedit">
  <div class="stackedit__left">
    <div class="stackedit__toc">
      
<ul>
<li><a href="#sessionとoldの違いについて">sessionとoldの違いについて</a></li>
<li><a href="#sessionとoldの性質について">sessionとoldの性質について</a></li>
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
<p>実際にstoreのHogeRulesが引っ掛かったときに帰ってくるデータはoldになるため、createでold取得するようにしています。</p>
<!-- session old&#12398;&#36949;&#12356;&#12395;&#12388;&#12356;&#12390;&#10;old&#12399;&#33258;&#20998;&#12363;&#12425;&#20195;&#20837;&#12377;&#12427;&#12371;&#12392;&#12364;&#12391;&#12365;&#12394;&#12356;&#12375;&#12289;&#12501;&#12521;&#12483;&#12471;&#12517;&#12392;&#12375;&#12390;&#12398;&#20596;&#38754;&#12434;&#25345;&#12387;&#12390;&#12356;&#12427;&#12290;&#10;session&#12399;&#12383;&#12384;&#27704;&#32154;&#30340;&#12395;&#20195;&#20837;&#12391;&#12365;&#12427;&#12384;&#12369;&#12391;&#12394;&#12367;$request-&gt;session()-&gt;flash($key, $value);&#12398;&#24418;&#12391;&#20195;&#20837;&#12377;&#12427;&#12371;&#12392;&#12364;&#12391;&#12365;&#12427;&#12290;&#10;&#12371;&#12398;&#12383;&#12417;&#12289;session&#12399;&#24133;&#24195;&#12356;&#12364;&#12289;old&#12399;&#38480;&#23450;&#30340;&#12395;&#12394;&#12427;&#12290;&#10;&#12383;&#12384;&#12375;&#12289;request&#12501;&#12457;&#12523;&#12480;&#12363;&#12425;&#12496;&#12522;&#12487;&#12540;&#12471;&#12519;&#12531;&#12363;&#12369;&#12383;&#22580;&#21512;&#12289;old&#12391;&#12375;&#12363;&#21463;&#12369;&#21462;&#12428;&#12394;&#12367;&#12394;&#12427;&#12290;&#10;&#10;&#12371;&#12398;&#28857;&#12434;&#32771;&#24942;&#12377;&#12427;&#12392;&#12289;old&#12399;&#19968;&#27010;&#12395;&#12356;&#12425;&#12394;&#12367;&#12394;&#12427;&#12431;&#12369;&#12391;&#12399;&#12394;&#12356;&#12290;&#10;&#12381;&#12428;&#12392;&#12289;old&#12399;&#21462;&#24471;&#12418;&#25163;&#36605;&#12391;&#12289;&#24341;&#25968;&#12394;&#12375;&#12384;&#12392;&#20840;&#20214;&#12289;&#12354;&#12427;&#12392;&#12381;&#12398;key&#12395;&#23550;&#24540;&#12375;&#12383;&#12418;&#12398;&#12384;&#12369;&#21462;&#24471;&#21487;&#33021;&#12290; -->

    </div>
  </div>
</body>

</html>
