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
end
```

- 