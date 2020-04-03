# DMM WEBCAMPコンテンツ【アプリケーションを完成させよう】第1章
DMM WEBCAMPの学習コンテンツアプリケーションを完成させよう第1章にて作成しました。

## 内容
**・アプリケーションのひな形を準備する**
Vagrantにて用意した仮想環境を起動、SSH接続後仮想環境に移動。
/home/vagrant/workに移動して下記、コードにて新規ディレクトリ（CARAVAN）作成。
`$ rails new CARAVAN`

**・モデルを設定する**
タイトル（title）カテゴリー（category）本文（body）の投稿機能をテーブルで設定するため
下記コードでモデルとテーブルを同時作成。
`rails g model Blog title:string category:string body:text`

**マイグレーションをデータベースに反映する**
モデル、テーブル、カラムを作成できたので、下記コードでデータベースに反映。
`$ rails db:migrate`
