## 総括
Ruby, PHP, Python はいずれも<br>
・スクリプト言語<br>
・サーバ側で動的にページを生成するMVCパターンを実現できるフレームワークがある<br>
ので、WEBアプリケーションをどの言語で作るかはプロジェクトや開発者の経験スキル次第か。<br>
<br>
オブジェクト指向プログラミング(クラスの概念がある)言語<br>
・Java, PHP, Ruby, Python -> いずれもクラスを利用👍<br>
・GoLang -> **クラスの代わりに構造体とメソッドを使用👍**<br>
<br>

## 動的型付け言語 と 静的型付け言語
① 動的型付け言語（JavaScript, Python, Ruby, PHP）<br>
② 静的型付け言語（Java, GoLang, C++)<br>
<br>
動的型付け言語ではあるが<br>
- JavaScriptはTypeScript導入で型導入可能。
- Pythonは型ヒントで 静的型付け要素を導入可能。
<br>
✅ 変数の型は【①プログラム実行時 ②コンパイル時】に決まる。<br>
✅ 変数の型を宣言する必要が【①ない ②ある】<br>
✅ コーディング時に型の整合性を取る必要が【①ない ②ある】<br>
✅ 実行時に型の不一致によるエラーが【①起きる ②起きない】<br>
<br>
スクリプト言語<br>
：コンパイルの手順がなく、ソースコードが直接実行環境によって解釈されます。<br>
コンパイル言語<br>
：事前にソースコードをコンパイル→実行可能なバイナリへ。あとは実行環境でバイナリを実行。<br>
<br>
👉動的/静的型付け言語は「型有無」の話、スクリプト/コンパイル言語は「コンパイル有無」の話。<br>

## 動作環境/サーバ
Java<br>
・JVMをインストールして、javaコマンド実行<br>
・Tomcat上でWEBサーバとして起動。<br>
<br>
PHP<br>
・PHPをインストールして、phpコマンド実行。<br>
・Apache/Nginxに拡張機能としてPHP実行環境をインストールしてWEBサーバとして起動。<br>
👉JSPのようにサーバ側で動的にページを生成可能(LaravelのMVCパターン)<br>
👉もちろんREST APIとしてもOK。<br>
<br>
Ruby<br>
・Rubyをインストールして、rubyコマンド実行。<br>
・Ruby専用のWEBサーバ(WEBrick,Thin,Puma)上で実行。<br>
👉RoR(Ruby on rails)ではDefaultサーバとして Puma が採用されている。<br>
👉JSPのようにサーバ側で動的にページを生成可能(Ruby on RailsのMVCパターン)<br>
👉もちろんREST APIとしてもOK。<br>
<br>
Python<br>
・Pythonをインストールして、pythonコマンド実行。<br>
・Python専用のWEBサーバ上で実行。WSGIかASGIに準拠するサーバ上で実行。<br>
・Browser <-> Apache/Nginx <-> WSGIサーバ のように 静的資源(html/css/js)はWEBサーバ、動的資源(REST API)はWSGIサーバの構成が一般的👉<br>
👉JSPのようにWSGIサーバ側で動的にページを生成可能(DjangoのMVCパターン)<br>
