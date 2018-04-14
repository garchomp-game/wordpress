---


---

<h1 id="sqlnot-null制約について">SQL:not null制約について</h1>
<p>先日は、会社で非常に興味深い話を聞くことができたので、一つ共有する。</p>
<p>私は今までmysqlぐらいしか触ったことなく、触ったことあるといっても本当に基本的なことぐらいしかわからない。</p>
<p>で、基礎すら危うい感じになってきたので、いったんどっとインストールで学びなおそうと思い、学習を進めていた。</p>
<p>そうしたら動画の通りに行かずに詰まったのが</p>
<blockquote>
<p>カラムの値がNULLだとはじかれてしまうため、途中で処理が止まる。</p>
</blockquote>
<p>という現象なんだが、これが全く違う挙動になってしまった。<br>
なんと<strong>warningだけ</strong>だしてそのまますべてのレコードが出てしまった。</p>
<p>それまでは、まともにsqlの設定ファイルをいじったことがなかったので、正直焦ってしまった。</p>
<p>同チームのSQL使いに質問をしてみたところ</p>
<p><strong>NOT NULL制約が入っていなかったから</strong><br>
とのことでした。</p>
<h1 id="sqlは全部同じとは限らない">SQLは全部同じとは限らない</h1>
<p>SQLにはいろいろな種類があり、SQLServer Oracle SQLite MySQL等があります。</p>
<p>それらのSQLは、それぞれ根本的な<strong>概念</strong>から大きく違います。</p>
<p>今回のNOT NULL制約について、明確に分かれているものは次の通りです。</p>

<table>
<thead>
<tr>
<th>SQL名</th>
<th>NOT NULL制約（デフォルト）</th>
</tr>
</thead>
<tbody>
<tr>
<td>SQLServer</td>
<td>あり</td>
</tr>
<tr>
<td>Oracle</td>
<td>なし</td>
</tr>
<tr>
<td>MySQL</td>
<td>あり(5.7から)</td>
</tr>
</tbody>
</table><p>このように、NOT NULLがデフォルトで入っているか入っていないかは、それぞれのSQLによっても変わってきます。<br>
が…実はこれらの設定は、手動で変えることができますので、本質的な話をすると、<strong>会社ごとに設定しているデフォルト値はそれぞれ違う</strong>ということをあらかじめ覚えておかなければなりません。</p>
<p>とはいえ、本来SQLそのものの基礎概念は全く異なっていて、身近な例で行くと</p>
<ul>
<li>デッドロックを起こさないように、not null制約をつけていない。</li>
<li>予期せぬデータが入っては絶対にならないので、not null制約をつけている。<br>
等があります。</li>
</ul>
<p>これらの制約、基礎概念について深く踏み込むとかなり難しくなってしまいますので割愛しますが、大事なのは</p>
<ul>
<li>目的によってどのSQLを使うべきなのか？</li>
<li>目的を達成するためにどうチューニングするべきなのか？<br>
を意識することが非常に大事になってきます。</li>
</ul>
<p>私自身、まだまだSQLは慣れていないためわからないこともたくさんありますが、少しずつ勉強を進めていきますが、やみくもに</p>
<blockquote>
<p>SQLiteでサイト完成したからいいや。</p>
</blockquote>
<p>と思わずに、本当にその選定が、そのチューニングがベストプラクティスなのか？をよく見直しておくことは大事です。</p>
<h1 id="まとめ">まとめ</h1>
<p>なんとなく、でデータベースを使っていたら、思わぬ事故を引き起こすことになります。<br>
なので、作業に入る前に、上流設計の地点で、データベースに関してよく検討することで、事前に対策を打っておきましょう。</p>
