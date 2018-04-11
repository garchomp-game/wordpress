

session oldについて


sessionとoldの違いについて
==================

どうも。エキセントリックエンジニアのおとかです。

さて、今回は、sessionとoldの違いについて紹介していくわけなんですが、なぜこれを紹介するかというと

* 検索してもなかなかピンとくる紹介例がない
* sessionはわかるけどoldって？て自分もなった
* とりあえずさくっと理解できる記事あるといいよね

て思ったので、今回まとめてみることにしました。

少しでも力になったら幸いです。

sessionとoldの性質について
==================

まずは、sessionとoldが一体どのように違うのでしょうか。以下のテーブルにまとめてみます。

session|old
-------------------------------------------- | ----------
代入できる|代入できない    
永続的に保存するようにもできるし、次のリクエストが来るまでの間のみ保存することもできる。 | 永続的に保存されない
ざっくりと主要なものはこの通りになります。
もともとsessionとoldは全くの別物…ではあるのですが、sessionには($request->session()->flash())があるし、結局同じフラッシュなんじゃないの？と混合した方もいると思いますが、全然そんなことはないです。

最も大きく違ってくるのは、例えばControllerからRequestフォルダ内にあるクラスをタイプヒントで渡している場合、はじかれた場合はoldとして取得することになります。
完全に独学でLaravelをやってきたので、実際のところの正しい書き方はわかりませんが…私の場合は、

``` php:hoge.php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;
use App\Http\Requests\HogeRules;
class CustomerController extends Controller
{
  public function index()
  {
    return view('about.index');  
  }

  public function create()
  {
    if (old()) {
      $array = old();
      unset($array['_token']);
      foreach ($array as $key => $value) {
        $request->session()->flash($key, $value);
      }
    }

    if (!(empty(session()->input('submit')))) {
      $request->session()->reflash();
    }
    return view('about.create');
  }

  public function store(HogeRules $request)
  {
    $array = $request->all();
    unset($array['_token']);
    unset($array['submit']);
    Comment::create($array());
    return redirect('about/');
  }
}
```
oldの配列の中身が空だったらfalseになるので、その部分を利用して、foreachで回してsessionに代入処理をしています。
また、更新対策として、reflashも掛けるようにしています。

実際にstoreのHogeRulesが引っ掛かったときに帰ってくるデータはoldになるため、createでold取得するようにしています。







<!-- session oldの違いについて
oldは自分から代入することができないし、フラッシュとしての側面を持っている。
sessionはただ永続的に代入できるだけでなく$request->session()->flash($key, $value);の形で代入することができる。
このため、sessionは幅広いが、oldは限定的になる。
ただし、requestフォルダからバリデーションかけた場合、oldでしか受け取れなくなる。

この点を考慮すると、oldは一概にいらなくなるわけではない。
それと、oldは取得も手軽で、引数なしだと全件、あるとそのkeyに対応したものだけ取得可能。 -->

