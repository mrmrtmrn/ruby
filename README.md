# プロを目指す人のためのRuby入門


## クラス
```ruby
class User
  # デフォルトの名前を定数として定義することもできる
  # 定数名は必ず大文字でスタートしないといけない
  DEFAULT_NAME = "DEFAULT_NAME"

  # 以下のようなメソッドを使うことによって個別のアクセスメソッドを用意しなくてよくなる
  # attr_accessor :name 読み書き用
  # attr_reader :name 読み取り用
  # attr_writer :name 書き込み用

  # 初期化関数
  def initialize(name = DEFAULT_NAME)
    # デフォルトでprivateなメソッドとして定義してあるため
    # cn = ClassName.new
    # cn.initializeのような呼び方をしてしまうとエラーになる

    # @がついている変数はRubyではインスタンス変数を意味する
    # 同じオブジェクト内で共有される変数になる
    @name = name
  end

  # クラスメソッド
  def self.create_users(names)
    # self.メソッド名とすることでクラスメソッドを定義することができる
    # 呼び出し方はUser.create_users(names)
    names.map do |name|
      User.new(name)
    end
  end

  # クラスメソッドを多く定義したい場合は以下のような書き方も可能
  # class << self
  #   def クラスメソッド
  #   end

  #   ...
  # end

  # インスタンスメソッド
  def hello
    "Hello, I am #{@name}."

    # 因みに...
    # 以下のように初期化されていないインスタンス変数にアクセスしてもエラーとならない
    # ※ローカル変数は初期化しておかないとエラーになる
    # "Hello, I am #{@full_name}."
  end

  # @nameを外部から参照するためのメソッド
  # attr_accessor: name もしくは
  # attr_reader :name で代替できる
  def name
    # インスタンス変数はクラスの外部からは参照することができないため
    # 参照用のメソッドを作る必要がある
    @name
  end

  # @nameを外部から変更するためのメソッド
  # attr_accessor: name もしくは
  # attr_writer :name で代替できる
  def name=(value)
    # インスタンス変数はクラスの外部からは参照することができないため
    # 変更用のメソッドを作る必要がある
    @name = value
  end

  # メモ化のテクニック
  # WebAPI使ったデータ取得などパフォーマンスに影響を与えるものに関しては
  # メモ化してしまって対応すれば一度のアクセスで使い回すことができる
  def twitter_data
    @twitter_data ||= begin
      # Twitter APIからデータを取得する処理を書く
    end
  end
end
```

## 継承
```ruby
class Product
  attr_reader :name, :price

  def initialize(name, price)
    @name = name
    @price = price
  end

  # Objectクラスに定義されているto_sメソッドのオーバーライド
  def to_s
    "name: #{name}, price: #{price}"
  end
end
```
```ruby
# DVDクラスはProductクラスを継承する
class DVD < Product
  # nameとpriceはスーパークラスでattr_readerが設定されているので定義不要
  attr_reader :runnning_time

  # def initialize(name, price, running_time)
  #   # スーパークラスにも存在している属性
  #   @name = name
  #   @price = price
  #   # DVDクラス独自の属性
  #   @running_time = running_time
  # end

  def initialize(name, price, running_time)
    # initializeメソッドのオーバーライド
    # ただし、スーパークラスのinitializeと全く同じ内容なのであれば特別定義する必要は無い
    # ※インスタンス生成時に自動的にスーパークラスのinitializeメソッドが呼び出されるため

    # スーパークラスの同名のメソッドを呼び出す(ここではinitializeメソッド)
    super(name, price)

    @running_time = running_time
  end

  # def initialize(name, price)
  #   # 引数をすべてスーパークラスのメソッドに渡す。super(name, price)と同じ
  #   super
  #   # サブクラスで必要な初期化処理を書く
  # end

  def to_s
    # superでスーパークラスのto_sメソッドを呼び出す
    "#{super}, running_time: #{running_time}"
  end
end
```
