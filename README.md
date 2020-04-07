# DMM WEBCAMPコンテンツ

# 【アプリケーションを完成させよう】第1章

## 〜モデルを設定する〜

**・モデル作成**
`$ rails g model モデル名`
**・モデルとテーブルを同時に作成**
`$ rails g model モデル名 カラム名1:データ型1 カラム名2:データ型2 ..`
`$ rails g model Blog title:string category:string body:text`
※短い文字列のカラムはstring型（一般的には255文字まで）、長い文字列のカラムにはtext型を使う（ほぼ無制限の長さで使用可能）。

**・モデル、テーブル、カラム作成完了後マイグレーションをデータベースに反映する。**
`$ rails db:migrate`


# 2章【TOPページを作ろう】
## 〜コントローラーとビューを設定する〜

**・コントローラーとビューを同時作成**
`rails g controller [コントローラ名] [アクション1] [アクション2]...`
`$ rails g controller blogs index show new edit`
下線：gはgenerate。コントローラやアクションをまとめて自動設定できる。
下線：コントローラの命名規則がある為blogsと複数形にする。

下記作成される
blogsコントローラ：controllersに追加
indexアクション：controllersに追加、routes.rb追加
showアクション：controllersに追加、routes.rb追加
newアクション：controllersに追加、routes.rb追加
editアクション：controllersに追加、routes.rb追加
blogs/index.html.erbファイル：viewsに追加（外観）
blogs/show.html.erbファイル：viewsに追加
blogs/new.html.erbファイル：viewsに追加
blogs/edit.html.erbファイル：viewsに追加


## 〜ルーティングを設定する〜

**・ルーティングに記述**
下記７つのアクションをRESTfulなURLと呼ぶ。resourcesメソッドを利用すると、このRESTfulなURLを自動で設定が可能。
index（記事一覧＋Topページ）
show（詳細表示ページ）
new（新規投稿ページ）
create（記事を保存する）
edit（記事編集ページ）
update（記事を更新する）
destroy（記事を削除する）

config/routes.rbで下記記述。
`resources :blogs`
※blogsは複数形かつコロンを先頭につける

記述後ルーティング（routes.rb）確認。
`$ rails routes`


## 〜HTML・CSSを記述する〜
**・HTMLを記述**
コントローラー作成時にindex.html.erbファイルが作成される。
上記ファイルに別で作成したHTMLのコードを記述。

**・CSSを記述**
app/assets/stylesheetsに別で作成したCSSのコードを記述。


**・application.html.erbにHTMLを記述**
各ページに共有して使われる部分（font-awesomeなど）はviews/layouts/application.html.erbファイルに記述。
各ViewのHTMLファイルが、<%= yield %>に読み込まれる。


## 〜画像用フォルダを作成する〜
画像ファイルはapp/assets/imagesフォルダに格納する。
（images配下にimgフォルダを作成してまとめると◯）


# 3章【投稿機能を作ろう】
## 〜createアクションを追加する〜
処理実行のみのアクション（create、update、destroy）はconfig/routes.rbでは設定したが
コントローラに作成していない為追加。
controllers/blogs_controller.rbフォルダに下記を記述。
`def create
end`
※newアクションの下に書くとわかりやすい。フォームから送られてきた投稿を、createアクションに保存する。


## 〜投稿フォームを表示する〜

ビュー配下にあるnew.html.erbファイルはビュー作成時に追加されている為、form_forを利用して、フォームを表示。
controllers/blogs_controller.rbフォルダのnewアクションに下記記述。
`@blog = Blog.new`
※新規データ登録用のblogインスタンスを、Blogモデルから作成。Blog.newと記述すると、空のモデルが生成される。

views/blogs/new.html.erbフォルダに記述。
￼


## 〜データを保存する〜

投稿フォームをデータベースに保存する処理を記述する。
form_forを使うときは、Strong Parametersの定義が必要。フォームからのデータを受け取れるようになる。
下記記述方法。
`params.require(:モデル名).permit(:カラム名1, :カラム名2, ...)`
controllers/blogs_controller.rbに記述。
￼

# 4章【Blog部分を書き換えよう
## 〜投稿データを取得する〜

Topページに対応するアクションは、indexアクションのため、controllers/blogs_controller.rbのindexアクションに投稿記事を取得する処理を追記。
Blogモデル内のすべての投稿記事データを取得するため下記記述変更。
`@blogs = Blog.all`


## 〜Topページ用のViewを作成する〜

Rubyタグのループ機能を用いて、取得した投稿データの一覧を自動表示させるためにviews/blogs/index.html.erbを記述変更。
※ループ処理に書き換え


# 5章【詳細ページを作ろう】
## 〜詳細ページを作成する〜

**・showアクションを作成**
showアクションを作成するため、controllers/blogs_controller.rbのshowアクション内に下記記述。データ取得後Viewに渡す。
`@blog = Blog.find(params[:id])`

**・投稿後に詳細ページへ遷移させる**

投稿後のリダイレクト先を詳細ページへ設定するため、controllers/blogs_controller.rbのcreateアクションに下記記述変更。
`blog = Blog.new(blog_params)
blog.save
redirect_to blog_path(blog.id) #この行を修正`

**・詳細ページ内容作成**
views/blogs/show.html.erbに詳細ページ内容を記述。


# 6章【編集機能を作ろう】
## 〜編集機能を追加する〜

**・editアクションを作成**
controllers/blogs_controller.rbのeditアクション内に下記記述。
`@blog = Blog.find(params[:id])`

**・編集フォーム作成**
views/blogs/edit.html.erbに編集フォーム内容を記述。

**・編集した記事の更新機能追加**
controllers/blogs_controller.rbにupdateアクション追加してその中に下記記述。
`def update
 blog = Blog.find(params[:id])
 blog.update(blog_params)
 redirect_to blog_path(blog)
end`

**・詳細画面に投稿の編集画面へ移動するリンクを追加**
views/blogs/show.html.erbに下記記述。
`<p class="single-text">
    <%= @blog.body %>    
</p>
<!-- 編集部分 -->
<%= link_to "編集", edit_blog_path(@blog) %>`


# 7章【削除機能を作ろう】
## 〜削除機能を追加〜

**・destroyアクションを作成**
controllers/blogs_controller.rbにdestroyアクションを追加してその中に下記記述。
`def destroy	
blog = Blog.find(params[:id])
blog.destroy
redirect_to blogs_path
end`

**・詳細画面に投稿の削除リンクを追加**
views/blogs/show.html.erbに下記記述。
`<%= link_to "削除", blog_path(@blog), method: :delete %>`




