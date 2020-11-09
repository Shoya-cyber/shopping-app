# Shopping-app

# 生活必需品を手軽に購入できる

# URL https://shopping-app-30527.herokuapp.com/

# テストアカウント　
  * 購入者: name: テスト１ email: test@com password: test1111
  * 出品者: name: テスト2  email: test2@com password: test2222
  * Basic認証　ID: admin password: 2222

# 利用方法
ユーザー登録画面にてユーザー情報、住所、クレジットカード情報を登録してトップページに移動する。
トップページから食品か生活雑貨を選択し商品一覧ページへ移動する。
購入したい商品と数量を選択し、カートへ入れる。
カート内の商品と購入金額を確認し購入する。
ユーザー詳細ページから購入履歴を確認できる。

# 目指した課題解決
高齢化社会やデジタル化が進む中で、インターネットが生活の一部にあることへの理解がより進むことはとても重要なことだと思います。
そこでショッピングサイトを利用することに抵抗がある方、ショッピングサイトを利用したことのない方に向けて,  
インターネットでの買い物の便利さや快適さを感じてもらうことを目的として制作しました。  

# 洗い出した要件
  * 事前ユーザー登録
    アプリの利用を簡潔にするため  
    事前に名前、アドレス、パスワード、住所、クレジットカード情報を登録してからトップページへ移動する
    
  * 商品一覧機能
    商品を一覧できる  
    食品または生活雑貨の商品一覧ページから商品をカートに入れることができる
    
  * 商品詳細ページ
  　商品の詳細を見ることができる  
    商品名、商品紹介文、値段をみることができる

  * カート機能
    複数の商品を購入できる  
    カートに入れた商品の一覧と合計金額を確認し、購入ページへ移動する

  * ユーザー詳細ページ
    登録情報、購入履歴を確認できる  
    登録情報に誤りや変更箇所はないか確認できる。購入履歴から再度購入するか、値動きはあるかなどを確認できる。

  * 出品機能
  　特定のユーザーは商品を出品できる  
    出品者は出品ページから商品を出品し、一覧表示ページに追加される


# テーブル設計

## users テーブル

|  Column     |  Type    |  Option       |
|  ---------  |  ------  |  -----------  |
|  name       |  string  |  null: false  |
|  name_kana  |  string  |  null: false  |
|  email      |  string  |  null: false  |
|  passward   |  string  |  null: false  | 

### Association

- has_one :address, dependent::destroy
- has_one :card, dependent::destroy
- has_many :orders, dependent::nullify

## orders テーブル

|  Column     |  Type        |  Option             |
|  ---------  |  ----------  |  -----------------  |
|  user       |  references  |  foreign_key: true  |
|  card       |  references  |  foreign_key: true  |
|  product    |  references  |  foreign_key: true  |
|  quantity   |  integer     |  null: false        | 
|  price      |  integer     |  null: false        | 

### Association

- belongs_to :user
- has_many :products, through::order_details
- has_many :order_details, dependent::destroy
- belongs_to :card
- belongs_to :address

## cards テーブル

|  Column     |  Type        |  Option             |
|  ---------  |  ----------  |  -----------------  |
|  user       |  references  |  foreign_key: true  |
|  order      |  references  |  foreign_key: true  |

### Association

- belongs_to :user
- has_one :order, dependent::nullify

## addresses テーブル

|  Column         |  Type        |  Option            |
|  -------------  |  ----------  |  ----------------  |
|  postal_code    |  string      |  null: false       |
|  prefecture_id  |  integer     |  null: false       |
|  address_line   |  text        |  null: false       |
|  phone_number   |  string      |  null: false       | 
|  user           |  references  |  foreign_key: true | 

### Association

- belongs_to :user
- has_one :order, dependent::nullify

## products テーブル

|  Column       |  Type     |  Option       |
|  -----------  |  -------  |  -----------  |
|  name         |  string   |  null: false  |
|  info         |  text     |  null: false  |
|  category_id  |  integer  |  null: false  |
|  price        |  integer  |  null: false  | 

### Association

- has_many :order_details
- has_many :orders, through::order_details
- has_many :line_items, dependent::destroy
- belongs_to :category

## categories テーブル

|  Column       |  Type        |  Option            |
|  -----------  |  ----------  |  ----------------  |
|  product      |  references  |  foreign_key: true |

### Association

- has_one :product

## carts テーブル

|  Column       |  Type     |  Option       |
|  -----------  |  -------  |  -----------  |
|  id           |           |               |

### Association

- has_many :line_items, dependent::destroy

## order_details テーブル

|  Column    |  Type        |  Option             |
|  --------  |  ----------  |  -----------------  |
|  product   |  references  |  foreign_key: true  |
|  order     |  references  |  foreign_key: true  |
|  quantity  |  integer     |  null: false        | 

### Association

- belongs_to :order
- belongs_to :product

## line_items テーブル

|  Column    |  Type        |  Option                   |
|  --------  |  ----------  |  -----------------------  |
|  product   |  references  |  foreign_key: true        |
|  cart      |  references  |  foreign_key: true        |
|  quantity  |  integer     |  null: false, default: 0  | 

### Association

belongs_to :product
belongs_to :cart


# ローカルでの動作方法
　git clone　https://github.com/Shoya-cyber/shopping-app.git

  rails 6.0.0 
  Ruby/Ruby on Rails/MySQL/Github/AWS/Visual Studio Code