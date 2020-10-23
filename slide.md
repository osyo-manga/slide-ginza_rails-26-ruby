#### 銀座Rails#26@リンクアンドモチベーション
- - -

### これからの Ruby と今の Ruby について

---

### 突然ですが！
#### Ruby 3.0 でこれを実行すると何が起きる？<br>private def value(value) = value => @value              <!-- .element: class="fragment" -->
#### → 知りたい人は最後まで読んでね！                       <!-- .element: class="fragment" -->


---

#### 自己紹介
- - -

* 名前：osyo
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* 趣味で Ruby にパッチを投げたり bugs.ruby で気になったチケットをブログにまとめたりしてる
    * Ruby 2.5 〜 2.7 までオレオレ機能を追加した
    * Ruby 3.0 では機能追加できなかった…
* Ruby で一番好きな機能は Refinements
* オンラインなのであちこちの地域.rb に参加してる                      <!-- .element: class="fragment" -->
<br>
    * [Fukuoka.rb](https://fukuokarb.connpass.com/)
[Hamada.rb](https://hamadarb.connpass.com/)
[kawasaki.rb](https://kawasakirb.connpass.com/)
[nikotama.rb](https://nikotamarb.connpass.com/)
[Sendai.rb](https://sendairb.connpass.com/)
[Shibuya.rb](https://shibuyarb.connpass.com/)
[Shinjuku.rb](https://shinjukurb.connpass.com/)
[Tama.rb](https://tamarb.connpass.com/)
[Toyama.rb](https://toyamarb.connpass.com/)
[Gotanda.rb](https://gotanda-rb.connpass.com/)
[Entaku.rb](https://entakurb.doorkeeper.jp/)
[Grow.rb](https://growrb.doorkeeper.jp/)
[Hamamatsu.rb](https://hmrb.doorkeeper.jp/)
[kanazawa.rb](https://kzrb.doorkeeper.jp/)
[Kobe.rb](https://koberb.doorkeeper.jp/)
[Machida.rb](https://machidarb.doorkeeper.jp/)
[mitaka.rb](https://mitakarb.doorkeeper.jp/)
[Ruby
Tuesday](https://ruby-tuesday.doorkeeper.jp/)
[toruby](https://toruby.doorkeeper.jp/)
[西日暮里.rb](https://nishinipporirb.doorkeeper.jp/)
などなど…
<br>
    * オンラインなので1日にで栃木→新宿→福岡に旅行ができて便利

---

### これからの Ruby と今の Ruby について

---

## これからの Ruby
## → Ruby 3.0                       <!-- .element: class="fragment" -->

---

## 注意
- - -

* このスライドでは基本的に Ruby 3.0.0-preview1 時点での挙動を元にして書いている                      <!-- .element: class="fragment" -->
    * 特別な記述がない限りは preview1 でコードを実行できる（はず
    * ただし、一部、最新版で動作確認しているので注意
* Ruby 3.0 がリリースされた時に挙動が変わっている可能性がある                      <!-- .element: class="fragment" -->
    * Ruby 3.0 を使用する場合はその時点でのリリースノート等の情報を参照してね

---

#### Ruby 3.0 の概要
- - -

* Ruby 3.0 は 2020/12/25 にリリース予定                  <!-- .element: class="fragment" -->
    * <del>オリンピックは延期しても Ruby のリリースは延期しないよ</del>
* <p class="fragment">それに先駆け先日 [Ruby 3.0.0 preview1](https://www.ruby-lang.org/en/news/2020/09/25/ruby-3-0-0-preview1-released/) がリリースされた</p>
* Ruby 3.0 はいろいろと目玉機能がてんこ盛りなのでみんな試してみよう！！                  <!-- .element: class="fragment" -->
    * 実際に試して変な挙動があればどんどん [bugs.ruby](https://bugs.ruby-lang.org/projects/ruby/wiki/HowtoReportJa) に報告してね
* 一方で Ruby 3.0 ではいくつか非互換な変更が入っている                  <!-- .element: class="fragment" -->
    * キーワード引数の挙動などなど…
    * バージョンを上げる際には注意する必要がある

---

#### Ruby 3.0 で追加される機能について解説

---

### RBS
- - -

* Ruby 3.0 で RBS という型情報の概念が追加される            <!-- .element: class="fragment" -->
* RBS は .rbs というファイルに型情報を定義する            <!-- .element: class="fragment" -->
    * Ruby のコード上には型情報は記述しない
    * .rbs の書き方はドキュメントをみてね
* RBS の利用方法は大きく分けて 2種類ある            <!-- .element: class="fragment" -->
    1. CLI から型情報を取得する
    2. Ruby のコード上から型情報を取得する
* また Ruby 3.0 時点では標準の機能で RBS を利用した型チェックなどは行わない予定            <!-- .element: class="fragment" -->
* RBS は gem としても配布されているので gem install rbs すれば Ruby 2.7 でも使えるよ！！            <!-- .element: class="fragment" -->

↓↓↓サンプル↓↓↓            <!-- .element: class="fragment" -->

>>>

* CLI から RBS を利用する
    * rbs コマンドから型情報を取得する

```
# 任意のクラス/モジュールの継承リストを取得する
$ rbs ancestors ::String
::String
::Comparable
::Object
::Kernel
::BasicObject
# 任意のメソッドの型シグネチャを取得する
$ rbs method ::String gsub
::String#gsub
  defined_in: ::String
  implementation: ::String
  accessibility: public
  types:
      (::Regexp | ::string pattern, ::string replacement) -> ::String
    | (::Regexp | ::string pattern, ::Hash[::String, ::String] hash) -> ::String
    | (::Regexp | ::string pattern) { (::String match) -> ::_ToS } -> ::String
    | (::Regexp | ::string pattern) -> ::Enumerator[::String, self]
```

>>>

* rbs コマンドで .rbs の雛形を生成する
* 元にする .rb ファイル

```ruby
# fizzbuzz.rb
class FizzBuzz
  def initialize(value)
    @value = value
  end
  def fizz?;     @value % 3 == 0 end
  def buzz?;     @value % 5 == 0 end
  def fizzbuzz?; fizz? && buzz?  end
  def to_s
      fizzbuzz? ? "FizzBuzz"
    : fizz?     ? "Fizz"
    : buzz?     ? "Buzz"
    : @value.to_s
  end
end

class Integer
  def to_fizzbuzz
    FizzBuzz.new(self)
  end
end
```

>>>

* "rbs prototype rb ファイル名" で雛形を生成できる
* このファイルを元に .rbs ファイルに型情報を記述する

```
$ rbs prototype rb fizzbuzz.rb
class FizzBuzz
  def initialize: (untyped value) -> untyped

  def fizz?: () -> untyped

  def buzz?: () -> untyped

  def fizzbuzz?: () -> untyped

  def to_s: () -> untyped
end

class Integer
  def to_fizzbuzz: () -> untyped
end
```

>>>

* こんな感じで実際の型情報を .rbs ファイルに保存する

```
# fizzbuzz.rbs
class FizzBuzz
  def initialize: (Integer) -> void

  def fizz?: () -> bool

  def buzz?: () -> bool

  def fizzbuzz?: () -> bool

  def to_s: () -> String
end

class Integer
  def to_fizzbuzz: () -> FizzBuzz
end
```

>>>

* Ruby から .rbs ファイルを読み込んで型情報を取得できる

```ruby
require "rbs"

loader = RBS::EnvironmentLoader.new()
# .rbs を読み込むパスを追加
loader.add(path: Pathname("./"))
environment = RBS::Environment.from_loader(loader).resolve_type_names
builder = RBS::DefinitionBuilder.new(env: environment)

# FizzBuzz クラスの型情報を取得
fizzbuzz = RBS::TypeName.new(name: :FizzBuzz, namespace: RBS::Namespace.root)
instance = builder.build_instance(fizzbuzz)

# 全インスタンスメソッドを出力
puts instance.methods.map { |name, sig|
  method.method_types.map { |sig| "FizzBuzz##{name}#{sig}" }
}
# output:
#  ...
#  FizzBuzz#fizz?() -> bool
#  FizzBuzz#buzz?() -> bool
#  FizzBuzz#fizzbuzz?() -> bool
#  ...

```

>>>

* 標準ライブラリの型情報を取得したりもできる

```ruby
require "rbs"

loader = RBS::EnvironmentLoader.new()
environment = RBS::Environment.from_loader(loader).resolve_type_names
builder = RBS::DefinitionBuilder.new(env: environment)

# String の型情報を取得する
string = RBS::TypeName.new(name: :String, namespace: RBS::Namespace.root)

# String#index の型シグネチャを取得する
instance = builder.build_instance(string)
puts instance.methods[:index].method_types.join("\n")
# output:
#  (::Regexp | ::string substr_or_regexp, ?::int offset) -> ::Integer?
#  (?::string str, ?capacity: ::int, ?encoding: ::encoding) -> ::String

# String.new の型シグネチャを取得する
singleton = builder.build_singleton(string)
puts singleton.methods[:new].method_types.join("\n")
# output:
#  (?::string str, ?capacity: ::int, ?encoding: ::encoding) -> ::String
```

>>>

#### 参照リンク
- - -

* [GitHub - ruby/rbs: Type Signature for Ruby](https://github.com/ruby/rbs)
    * [rbs/CONTRIBUTING.md at master · ruby/rbs · GitHub](https://github.com/ruby/rbs/blob/master/docs/CONTRIBUTING.md)
* [rbs cli - pockestrap](https://pocke.hatenablog.com/entry/2020/06/15/081130)
* [Ruby の型関連の情報まとめ - Qiita](https://qiita.com/natsuokawai/items/6d652e3b72a79e019f22)

>>>


---

### TypeProf
- - -

* TypeProf は Ruby のソースコードから型推論して型情報がある .rbs を生成するツール          <!-- .element: class="fragment" -->
    * 最近 TypeProfiler から TypeProf に名前が変わった
* TypeProf は Ruby 3.0 に同梱される予定                <!-- .element: class="fragment" -->
    * 開発版だとすでに [bundled gem になっている](https://github.com/ruby/ruby/pull/3668)
* これは .rb をパースする際に実際にメソッドに渡されたリテラルから型を推論するような仕組みになっている               <!-- .element: class="fragment" -->
    * なので .rb の書き方によって生成される .rbs は異なる
* また TypeProf は Ruby 3.0.0-preview1 には同梱されていないので注意               <!-- .element: class="fragment" -->
* TypeProf は gem としても配布されているので gem install typeprof で今すぐ試すことができる！！               <!-- .element: class="fragment" -->

↓↓↓サンプル↓↓↓               <!-- .element: class="fragment" -->

>>>

* typeprof コマンドで .rbs 形式の情報を出力する
* Ruby 3.0.0-preview1 では事前に gem install typeprof しておく必要がある
    * ここに書かれているのは 2020/10/21 時点のもの

```ruby
# 元ファイル
# twice.rb
def twice(a)
  a + a
end
```

```
# typeprof コマンドで .rbs 形式の出力がされる
# typeprof は実際にメソッドに渡される引数リテラルを元にして型推論を行う
# 現状だと twice メソッドの呼び出しがないので型推論できない…
$ typeprof twice.rb
# Classes
class Object
  def twice : (untyped) -> untyped
end
```

>>>

* TypeProf はメソッド呼び出しのリテラルから型推論する

```ruby
# twice.rb
def twice(a)
  a + a
end

# メソッドを呼び出す
twice(42)
twice("homu")
```

```
$ typeprof twice.rb
# 型情報が追加されている
# twice に Integer や String が渡されている情報を元にして型推論する
# Classes
class Object
  def twice : (Integer | String) -> (Integer | String)
end
```

>>>

* 変数やメソッドを経由しても推論される、すごい

```ruby
def twice(a)
  a + a
end

value = 42
value2= twice(value) # Integer を渡す
twice(value2.to_s)   # String を渡す
```

```
$ typeprof twice.rb
# Classes
class Object
  def twice : (Integer | String) -> (Integer | String)
end
```

>>>

* 複雑なコードを渡すと…？

```ruby
class FizzBuzz
  def initialize(value)
    @value = value
  end
  def fizz?;     @value % 3 == 0 end
  def buzz?;     @value % 5 == 0 end
  def fizzbuzz?; fizz? && buzz?  end
  def to_s
      fizzbuzz? ? "FizzBuzz"
    : fizz?     ? "Fizz"
    : buzz?     ? "Buzz"
    : @value.to_s
  end
end
class Integer
  def to_fizzbuzz
    FizzBuzz.new(self)
  end
end
puts 1.upto(20).map { |index|
  index.to_fizzbuzz
}
```

>>>

* こういう結果になる
* かなり便利そうなので期待したい

```
$ typeprof fizzbuzz.rb
# Classes
class Integer < Numeric
  def to_fizzbuzz : -> FizzBuzz
end

class FizzBuzz
  @value : Integer
  def initialize : (Integer) -> Integer
  def fizz? : -> bool
  def buzz? : -> bool
  def fizzbuzz? : -> bool
  def to_s : -> String
end
```

>>>

#### 参照リンク
- - -

* [GitHub - ruby/typeprof: An experimental type-level Ruby interpreter for testing and understanding Ruby code](https://github.com/ruby/typeprof)
* [typeprof/doc.ja.md at master · ruby/typeprof · GitHub](https://github.com/ruby/typeprof/blob/master/doc/doc.ja.md)
    * 日本語の資料なので読みやすい！
    * ここを読めばだいたい OK
* [[JA] Type Profiler: a Progress Report of a Ruby 3 Type Analyzer / Yusuke Endoh @mametter - YouTube](https://www.youtube.com/watch?v=6KcFdQWp8W0)
    * RubyKaigi Takeout 2020 の登壇動画
* [Type Profiler: Ambitious Type Inference for Ruby 3](https://www.slideshare.net/mametter/type-profiler-ambitious-type-inference-for-ruby-3-238384550)
    * RubyKaigi Takeout 2020 の登壇資料


---

### Ractor
- - -

* Ractor は Ruby で並列処理を行うためのライブラリ       <!-- .element: class="fragment" -->
    * 以前は Guild という名前で開発されていたが Ractor という名前に変わった
    * Ruby + Actor の略
* Ruby 3.0 では実験的に導入される予定       <!-- .element: class="fragment" -->
* Ractor に関しては日々改良されているので preview1 と比較して Ruby 3.0 がリリースされる頃にはいろいろと使い方や制限が変わっている可能性があるので注意する       <!-- .element: class="fragment" -->
    * 最近も `#recv` から `#receive` に名前が変わった
    * サポートする共有可能オブジェクトが追加されたりとか

↓↓↓サンプル↓↓↓       <!-- .element: class="fragment" -->

>>>

* 以下のコードを実行すると hello と world が混ざって出力される

```ruby
# Ractor.new のブロックが並行処理として実行される
# ブロック内は外の変数を参照できないので
# .new の引数がブロックの引数として受け取る事ができる
ractor = Ractor.new(10) do |loop_count|
  # トップレベルとは別に Ractor 内でも設定する必要がある
  $stdout.sync = true

  loop_count.times do
    puts :hello
    sleep 0.3
  end
end

# 出力バッファリングを無効にする
$stdout.sync = true

10.times do
  puts :world
  sleep 0.3
end
```

>>>

* Ractor のメッセージの送り方：push 型

```ruby
# Ractor へメッセージを送る
# push 型
ractor = Ractor.new {
  $stdout.sync = true

  puts "=== Ractor start ==="
  # メッセージを受け取るまで待つ
  # 将来的に recv は receive に名前が変わるので注意
  value = Ractor.recv
  puts value  # => 42
  puts "=== Ractor end ==="
  sleep 3

  # ブロックの戻り値が take の戻り値になる
  value + value
}

sleep 3
ractor.send(42)

# 待ち処理
puts ractor.take  # => 84
puts "=== Finish ==="
```

>>>

* Ractor のメッセージの送り方：pull 型

```ruby
# Ractor からメッセージを受け取る
# pull 型
ractor = Ractor.new {
  $stdout.sync = true

  puts "=== Ractor start ==="
  # take が呼ばれるまで待つ
  puts Ractor.yield 42
  puts "=== Ractor end ==="
}

sleep 3

# 待ち処理
# Ractor.yield の引数を返す
puts ractor.take  # => 42
sleep 2
puts "=== Finish ==="
```

>>>

* Ractor でスリープソートを書いてみるとこんな感じ
    * 要素の値だけ sleep して要素の値が少ない順で処理するソート

```ruby
$stdout.sync = true

puts "かいし！！！"
data = (1..20).to_a.shuffle
p data

ractors = data.map { |i|
  Ractor.new(i) { |it|
    # バッファリングを無効化
    $stdout.sync = true

    sleep it / 5.0
    puts it
  }
}

# 待ち処理
ractors.map(&:take)

puts "おわり！！"
```

>>>

#### 参照リンク
- - -

* [ruby/ractor.md at master · ruby/ruby · GitHub](https://github.com/ruby/ruby/blob/master/doc/ractor.md)
* [ruby/ractor.ja.md at ractor · ko1/ruby · GitHub](https://github.com/ko1/ruby/blob/ractor/ractor.ja.md)
    * 日本語の資料だがちょっと古いので注意
* [Ractor超入門 - Qiita](https://qiita.com/S_H_/items/f9d1917fda770bcdbe2a)
* [並列処理入門 + Rubyでの新しい並列実行単位Ractor - Qiita](https://qiita.com/tomboyboy/items/dc0db552088923d9ce6b#ractor%E3%81%AE%E7%8F%BE%E5%9C%A8)

---

#### Module#include の挙動が変わった
- - -

* Ruby 3.0 で Module#include / prepend の挙動がちょっと変わります         <!-- .element: class="fragment" -->
* 今まではすでに include 済みのモジュールに対して include してもすでに include されているオブジェクトには反映されませんでした       <!-- .element: class="fragment" -->
* Ruby 3.0 ではあとから include した場合でもすでに include 済みのオブジェクトに反映されるようになりました       <!-- .element: class="fragment" -->
* 何をいってるのかわからないと思うのでサンプルコードを見てみよう       <!-- .element: class="fragment" -->


↓↓↓サンプル↓↓↓       <!-- .element: class="fragment" -->

>>>

* 以下のようすでに X.include M してる M に対して M.include M2 をすると X.ancestors に M2 が反映されるようになる

```ruby
module M; end

class X
  include M
end
# この時点では M のみ反映されている
p X.ancestors
# => [X, M, Object, Kernel, BasicObject]

module M2; end

# M に対してあとから include したときの挙動が変わった
M.include M2

# Ruby 2.7 : M2 は反映されない
# Ruby 3.0 : M2 が反映される
p X.ancestors
# Ruby 2.7 => [X, M, Object, Kernel, BasicObject]
# Ruby 3.0 => [X, M, M2, Object, Kernel, BasicObject]
```

>>>

* Ruby 3.0 からは Kernel.include M をすると既存のクラスに反映される

```ruby
module M
  def twice
    self + self
  end
end

p String.ancestors

Kernel.include M
# => [String, Comparable, Object, Kernel, BasicObject]

# 既存のクラスに M が反映されるようになる
p String.ancestors
# Ruby 2.7 => [String, Comparable, Object, Kernel, BasicObject]
# Ruby 3.0 => [String, Comparable, Object, Kernel, M, BasicObject]

# M のインスタンスメソッドが呼び出せるようになる
p "hoge".twice
# Ruby 2.7 => error: undefined method `twice' for "hoge":String (NoMethodError)
# Ruby 3.0 => "hogehoge"
```

>>>

* またこれは以下のように複数のモジュールを mixin している場合に問題になる可能性がある

```ruby
module M1
  def to_s; "M1 -> " + super end
end

module M2
  def to_s; "M2 -> " + super end
end

class X
  include M1
  include M2

  def to_s; "X -> " + super end
end

# このあたりは今までどおり期待する挙動
p X.ancestors
# => [X, M2, M1, Object, Kernel, BasicObject]

p X.new.to_s
# => "X -> M2 -> M1 -> #<X:0x000055977aa63e68>"
```

>>>

* 以下のようにすると .ancestors に同じモジュールが複数含まれるようになる
* これにより今までの挙動と少し変わる可能性がある
* 今までは問題なかったが Ruby 3.0 に上げるとモジュールが重複してしまう可能性があるので注意する

```ruby
# M1 に M2 を mixin する
M1.include M2

# Ruby 2.7 だとモジュールは重複しないが Ruby 3.0 だと重複するようになる
p X.ancestors
# Ruby 2.7 => [X, M2, M1, Object, Kernel, BasicObject]
# Ruby 3.0 => [X, M2, M1, M2, Object, Kernel, BasicObject]

# 当然 super 経由で呼び出されるメソッドも異なるようになる
p X.new.to_s
# Ruby 2.7 => "X -> M2 -> M1 -> #<X:0x000055977aa63e68>"
# Ruby 3.0 => "X -> M2 -> M1 -> M2 -> #<X:0x000055b8645d1d40>"
```


>>>

#### 参照リンク
- - -

* [Ruby 3.0 で変わる Module#include の挙動 - Secret Garden(Instrumental)](https://secret-garden.hatenablog.com/entry/2020/09/04/163454)
* [[Bug #9573] descendants of a module don't gain its future ancestors, but descendants of a class, do](https://bugs.ruby-lang.org/issues/9573)
    * 元のバグチケット
* [[Bug #17038] On master, ancestry edits can lead to duplicates in Module#ancestors](https://bugs.ruby-lang.org/issues/17038)
    * 継承リストに同じモジュールが複数含まれるのはバグじゃない？というバグチケット
    * 実際はバグではなくて意図した挙動なのでこのチケットは Reject されている


---

### 右代入
- - -

* 通常は左辺に変数に対して右辺の値を代入する       <!-- .element: class="fragment" -->
    * result = 42
* 右代入は左辺の値を右辺の変数に代入する構文       <!-- .element: class="fragment" -->
    * 42 => result
* これを利用するとメソッドチェーンなど長いコードを書いている際に左から右にコードを書く流れで変数を定義する事ができる       <!-- .element: class="fragment" -->
    * irb などで変数定義する場合にいちいち左にカーソルを移動させたりしていたがそういう煩わしさがなくなる
* ただし、いくつか注意点がある       <!-- .element: class="fragment" -->
* Ruby 3.0 では実験的な機能という位置づけ       <!-- .element: class="fragment" -->

↓↓↓サンプル↓↓↓       <!-- .element: class="fragment" -->

>>>

* 通常は左辺の変数に右辺の値を代入していた

```ruby
# 左辺に右辺の値を代入
result = 42
```

* Ruby 3.0 から追加される => 演算子を使うと『左辺の値を右辺の変数に』代入する事ができる

```ruby
# 左辺を右辺に代入
42 => result

# 多重代入などもできる
[1, 2] => a, b
```

* ただし、以下のような代入はできないので注意

```ruby
# error
# see: https://bugs.ruby-lang.org/issues/17190
1, 2 => a, b
```

>>>

* メソッドの戻り値を代入する場合に注意する必要がある
* => の優先順位の問題で以下のコードはうまく動作しない

```ruby
def twice(a)
  a + a
end

# twice(42) の結果を変数 reuslt に代入したい
twice 42 => result
```


* この書き方は twice({ 42 => result }) と解釈される
* 右代入は処理の優先順位を考えて使う必要がある

```ruby
# これは => が Hash リテラルとして解釈される為
# twice 42 => result という書き方は Ruby 2.7 でも有効な書き方になる
twice({ 42 => result })

# 明示的に括弧を付けて代入する
twice(42) => result
p result # => 42
```

>>>


#### 参照リンク
- - -

* [改めて整理する Ruby 3.0 に実験的に入る予定のエンドレスメソッド定義構文と右代入演算子について - Secret Garden(Instrumental)](https://secret-garden.hatenablog.com/entry/2020/10/03/180130)

---

### エンドレスメソッド定義
- - -

* 1行でメソッドが定義できる構文              <!-- .element: class="fragment" -->
* def value() = 42 みたいに定義できる          <!-- .element: class="fragment" -->
    * エンドレスとは『end がない』という意味
* Ruby 3.0 では定義できるメソッドの仕方に制限があるので注意する    <!-- .element: class="fragment" -->
* 元々はエイプリルフールネタで投稿されたチケットがなぜか実装されてしまい、その流れで入った    <!-- .element: class="fragment" -->
* Ruby 3.0 では実験的な機能という位置づけ       <!-- .element: class="fragment" -->

↓↓↓サンプル↓↓↓          <!-- .element: class="fragment" -->

>>>

* def メソッド名() = 式という形でメソッドを定義することができる

```ruby
class FizzBuzz
  def initialize(value) = @value = value

  def fizz?() = @value % 3 == 0
  def buzz?() = @value % 5 == 0
  def fizzbuzz?() = fizz? && buzz?

  def to_s()
    = fizzbuzz? ? "FizzBuzz"
    : fizz?     ? "Fizz"
    : buzz?     ? "Buzz"
    : @value.to_s
end

class Integer
  def to_fizzbuzz() = FizzBuzz.new(self)
end

puts 1.upto(20).map { |index|
  index.to_fizzbuzz
}
```

>>>

* 通常のメソッド定義とは違いいくつか制限がある
* 必ず () を書く必要があり = が付いているメソッドは定義できない

```ruby
# OK
def set_value(value) = @value = value
def value() = @value
def invalid?() = @value.nil?


# NG
# () を付ける必要がある
# syntax error, unexpected '=', expecting ';' or '\n'
def value = 42

# 名前に = がついてるメソッドは定義できない
# error: setter method cannot be defined in an endless method definition
def value=(value) = @value = value
```

>>>


#### 参照リンク
- - -

* [改めて整理する Ruby 3.0 に実験的に入る予定のエンドレスメソッド定義構文と右代入演算子について - Secret Garden(Instrumental)](https://secret-garden.hatenablog.com/entry/2020/10/03/180130)


---

### 右代入とエンドレスメソッド定義を
### 組み合わせると
### 何が起きる…？

---

* 例えばセッターメソッドとしてこういうメソッドを定義する

```ruby
private def set_value(value)
  @value = value
end
```

* これをエンドレスメソッド定義するとこうなる

```ruby
private def set_value(value) = @value = value
```

* 更に右代入を利用するとこうなる

```ruby
private def set_value(value) = value => @value
```

---

### どうなるのか 1つ1つ見ていく

---

まず

```ruby
def value(value) = value => @value
```

は

```ruby
# def 式の戻り値が @value に代入される
(def value(value) = value) => @value
@value # => :value
```

となる
<br>
これは右代入よりもメソッド定義のほうが優先順位が高いから

---

更に

```ruby
private def value(value) = value => @value
```

は

```ruby
private(def value(value) = value => @value)
```

という意味になる

---

つまり

```ruby
private def value(value) = value => @value
```

は

```ruby
# (def value(value) = value) がキーで値が @value の Hash を private に渡そうとする
# error: `private': {:value=>nil} is not a symbol nor a string (TypeError)
private( { (def value(value) = value) => @value } )
```

と同じ意味になる
<br>
右代入が => を使っているのがだいたい悪い

---

## Ruby むずかしくない？？？

---

#### その他：細かい変更点や追加点
- - -

* キーワード引数周りで非互換な変更が入る          <!-- .element: class="fragment" -->
    * 今まで出てた警告がエラーになる
* Hash#except, ENV.except が追加された          <!-- .element: class="fragment" -->
    * ほしかったやつ
* Symbol#name が追加された          <!-- .element: class="fragment" -->
    * 自身を frozen string で返すメソッド
* _1 という名前の変数やメソッドは定義できなくなる          <!-- .element: class="fragment" -->
* (...) 引数で第一引数を仮引数で定義できるようになった          <!-- .element: class="fragment" -->

```ruby
def method_missing(meth, ...)
  send(:"do_#{meth}", ...)
end
```
<!-- .element: class="fragment" -->

* <p class="fragment">詳しくは [Ruby 3.0.0-preview1 の NEWS](https://github.com/ruby/ruby/blob/master/NEWS.md) をみてくれよな！</p>


---


## 今の Ruby
## → Ruby 2.7.2            <!-- .element: class="fragment" -->

---

### Ruby 2.7.2 でどういう世界になるのか

---

#### Ruby 2.7.2 の世界
- - -

* <p class="fragment">先日 [Ruby 2.7.2 がリリース](https://www.ruby-lang.org/ja/news/2020/10/02/ruby-2-7-2-released/)された</p>
* このリリースでは webrick の脆弱性対応が含まれている                  <!-- .element: class="fragment" -->
* また『非推奨な警告がデフォルトでは出力さなくなる』という対応も含まれている                  <!-- .element: class="fragment" -->
* 今回話すのはこの『非推奨な警告がデフォルトでは出力さなくなる』ことに対して                  <!-- .element: class="fragment" -->

---

#### 非推奨な警告が出力されなくなるとどうなる？
- - -

1. アプリケーションで非推奨な機能を使っていた                  <!-- .element: class="fragment" -->
    * これは今まで問題なかったコードが非推奨になってしまうケースも含まれる
1. 非推奨であることに気づかないままそのコードを使い続けた                  <!-- .element: class="fragment" -->
    * デフォルトでは警告が出なくなるので気づきづらい
1. ある日、非推奨な機能が Ruby から削除された                  <!-- .element: class="fragment" -->
1. そして Ruby のバージョンを上げると突然アプリケーションが壊れる                  <!-- .element: class="fragment" -->

<br>
## これはこわい！！                  <!-- .element: class="fragment" -->

---

#### 明示的に非推奨な警告を出力する方法
- - -

* 明示的に非推奨な警告を出力する手段として 2つある                <!-- .element: class="fragment" -->
* 1つは Ruby のコマンドラインオプションに -W:deprecated を渡す                <!-- .element: class="fragment" -->
    * ruby -W:deprecated sample.rb
* もう1つは Ruby のコード上で Warning[:deprecated] = true を呼び出す                <!-- .element: class="fragment" -->
* この2つは Ruby 2.7 から使える機能になる                <!-- .element: class="fragment" -->
    * なので Ruby 2.6 とかで使おうとするとエラーになる
    * Ruby 2.6 の環境と混同する場合はワークアラウンド対応が必要…

---

### これからどう向き合っていくのか

---

### いつ警告を出す？
- - -

* どのタイミング、どの出力で警告を出すべき？                <!-- .element: class="fragment" -->
* CI で出す？                <!-- .element: class="fragment" -->
* ローカルで出す？                <!-- .element: class="fragment" -->
* テストを実行したときに出す？                <!-- .element: class="fragment" -->
* rails s で出す？                <!-- .element: class="fragment" -->
* などなどいろいろと考えられる                <!-- .element: class="fragment" -->

---

### 警告を出すべき出さないべき？
- - -

* そもそも警告を出すべき？出さないべき？                <!-- .element: class="fragment" -->
* 警告を出さない場合は非推奨な機能が削除された場合にいきなりアプリケーションが壊れてしまう                <!-- .element: class="fragment" -->
* 警告自体はいずれかのタイミングでキャッチアップするべき？                <!-- .element: class="fragment" -->
* 普段は気にせずに開発して Ruby のバージョンを上げる際に対応する手もある？                <!-- .element: class="fragment" -->
    * でも漏れがあったら怖いし…

---

### どうやって制御する？
- - -

* コマンドオププションで制御する？Warning[:deprecate] で制御する？                <!-- .element: class="fragment" -->
* 環境変数 RUBYOPT に -W:deprecated を追加すればいい？                <!-- .element: class="fragment" -->
    * でも Ruby 2.6 でこのオプションを参照すると死ぬ…
    * Ruby 2.7 のプロジェクトでのみ RUBYOPT に追加したい
* Warning[:deprecate] = true する場合どのタイミンで呼び出す？                <!-- .element: class="fragment" -->
    * Rails のプロジェクトだと ./bin/rails に直接記述してしまうとか？
* gem を開発する場合 Warning[:deprecate] を書き換えて運用するのは難しそう…                <!-- .element: class="fragment" -->

---

## 考えることが多くて結構難しい

---

### 個人的な結論
- - -

* いろいろとあって Ruby 2.7.2 ではデフォルトで出なくするという選択肢を選んだ                <!-- .element: class="fragment" -->
* しかし、今まではデフォルトで警告が出ること前提で運用してきた                <!-- .element: class="fragment" -->
* なのでいずれにしろ警告自体を出すようにする事自体は問題ないはず                <!-- .element: class="fragment" -->
* その上でどういう手段を用いて警告を出すようにするのか考えていく必要がある                <!-- .element: class="fragment" -->
* 他にもいい運用の仕方を思いついたらどんどんフィードバックしてほしい                <!-- .element: class="fragment" -->
* 個人的にはアプリケーションコードの警告は出力して、gem の警告は出力しないようにするなど細かい制御をしていきたい                <!-- .element: class="fragment" -->
    * warning-gem とか試してみたい

### いい運用方法があったら教えて！！               <!-- .element: class="fragment" -->

---

## まとめ

---

#### まとめ
- - -

* Ruby 3.0.0 preview1 が出たからみんな試してみてね                <!-- .element: class="fragment" -->
    * バグとかあったらフィードバックがほしい
    * rbs や typeprof は gem install でシュッと試せるよ！
* Ruby 2.7.2 から非推奨な警告がデフォルトで出なくなるよ                <!-- .element: class="fragment" -->
    * どう運用していくのか考えるとむずかしい
    * 警告自体は問答無用で出してしまってしまうのがよいと思う
    * どう運用していくのがいいのかフィードバックがほしい
* Ruby 3.0 はまだ開発中なので実際にリリースされる時に挙動が変わっている可能性があるので注意しましょう                <!-- .element: class="fragment" -->
* オンライン勉強会は場所を選ばず参加できるので参加し得                <!-- .element: class="fragment" -->
    * 今の時期しかできないよ！！

---

## ご清聴
## ありがとうございました

