コントローラーからの呼び出し処理で使われるviewとredirect。実はあまり違いを理解できていない…なんてことはありませんか？

今回は混合しがちなviewとredirectについて違いをまとめていきます。

# viewとredirectの紹介
viewは、呼び出されたとび先のビューファイルを直接呼び出します。
一方、redirectは、ビューを直接呼び出すのではなく、getで指定先のアクションに飛んでいます。

```php sample.php
<?php 

class MainController extends Controller
{
  public function create()
  {
    // viewはビューファイルを直接読んでいる。
    return view('main.create');
  }

  public function conf(Request $request)
  {
    if ($request->all()) {
      return view('main.conf')->with('postdata' => $request->all());
    } else {
      // ここではmainコントローラーのcreateアクションを読んでいる。
      return redirect ('main/create')->withInput();
    }
  }
}
```
confアクションに注目してください。今回は$request->all()の中身があった場合としていますが、本来この条件分岐に、何らかのバリデーションや条件が入ります。

パスしたらそのままconf画面の**ビューファイルを呼び出し**、パスしなかったら、createアクションに**リダイレクトして**います。

これらの法則をマスターしておくことで、より表現力の高いアプリを組むことが出来るようになります。
