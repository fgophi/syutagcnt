# syutagcnt
FGO周回カウンタの報告を集めExcel出力する

# 概要
Twitter とYahoo!リアルタイム検索から7から10日前までの #FGO周回カウンタ のデータを集めExcel出力する

# 必要環境
* Twitter のアカウント (アプリ連携が必要)
* Python が動作する環境
* ~~Google Chrome~~ 2020/4月のYahoo!リアルタイム検索のデザイン変更により動作しない

# ファイル
* syutagcnt.py
* freequest.csv フリクエの定義ファイル
* syurenquest.csv 修練場の定義ファイル
* item.csv アイテム(素材)の定義ファイル
* quest.csv クエスト名の変換ファイル
* setting-dst.ini setting.ini を作成するための元ファイル

# インストール
1. Python のインストール
2. Pythonパッケージ一括インストール
3. ファイルのコピー
4. setting.ini の作成
5. Twitter 認証
6. ~~使用するChromeのバージョンに応じたSeernium のWebDriverのインストール~~
***
1. Python のインストール
https://www.python.org/
からPython をダウンロードしてインストール

2. Pythonパッケージ一括インストール  
コマンドラインから  
`$ pip install -r requirements.txt`  
を実行 (pip はPyrhon に含まれるコマンド)

3. ファイルのコピー
以下のファイルを使用するフォルダにコピーする。
*	syutagcnt.py
*	freequest.csv
*	syurenquest.csv
*	item.csv
*	quest.csv

4. setting.ini の作成  
setting-dst.ini を setting.ini という名前でコピーする
例: `$ copy setting-dst.ini setting.ini`

https://developer.twitter.com/ からアプリケーション登録をしてconsumer_keyとconsumer_secretを手に入れた consumer_keyとconsumer_secretを setting.ini の該当箇所に記述する。ここでaccess_token と access_secret も手に入れて記述しておくと 5.の手順はとばせる

5. Twitter 認証  
コマンドプロンプトから  
`$ python syutagcnt.py`  
を実行

ウェブブラウザが開いて下記の画面がでるので「連携アプリを認証」を選択
![syutagcnt1](https://user-images.githubusercontent.com/62515228/78854315-f50b6a00-7a5b-11ea-9a64-12f57a41274a.png) 

すると下記のようなPINコードがでるのでこの数字をコピペして
![syutagcnt2](https://user-images.githubusercontent.com/62515228/78854461-59c6c480-7a5c-11ea-8448-9906c484a7ce.png) 

    次のURLをウェブブラウザで開きます: https://api.twitter.com/oauth/authorize?oauth_token=***********************************
    oauth_token: *************************************
    ウェブブラウザに表示されたPINコードを入力してください:

と入力を待ちのコマンドプロンプトにペーストしてEnterを押す

    Twitterのアプリ認証は正常に終了しました。

とでれば準備完了

~~6.	使用するブラウザに応じたSeernium のWebDriverのインストール~~
~~まず、 http://chromedriver.chromium.org/downloads より、今使用しているChrome のバージョンに合わせたファイル(Windowsの場合はchromedriver_win32.zip)をダウンロードする.。~~
~~ダウンロードしたファイルを展開し、chromedriver.exeをyahoocnt.py のあるフォルダ(またはPATHの通ったフォルダ)に入れる。~~
 

# 使い方
    usage: syutagcnt.py [-h] [-y] [-c] [-r] [-u URL] [-a] [-f] [-n] [-w WAIT]
                        [--version]
                        filename
    
    FGO周回カウンタの報告を集めExcel出力する
    
    positional arguments:
      filename              出力Excelファイル名
    
    optional arguments:
      -h, --help            show this help message and exit
      -i, --ignoreclass     クラス無しをエラーを無視する
      -y, --yahoo           Yahoo!リアルタイム検索からデータを取得する
      -c, --checkreply      リプライデータをチェックする
      -r, --resume          前回取得したツイートの続きから取得
      -u URL, --url URL     指定したURLより未来のツイートを取得
      -a, --asc             出力を昇順にする
      -f, --nofavorited     自分がふぁぼしていないツイートのみを取得
      -n, --number          統計シートの報告にNoをつける
      -w WAIT, --wait WAIT  ブラウザ操作時の待ち時間(秒)を指定する(デフォルト2秒)
      --version             show program's version number and exit

# 出力データ
* 「全データ」タブ:  
検索された #FGO周回カウンタ タグ(リツイートを除く)
* 「フリクエ1部」「フリクエ1.5部」「フリクエ2部」「修練場」タブ:   
振り分けられたツイート
「その他クエスト」タブ: 
上記に当てはまらなかったツイート。イベントなどはここに入る
* 「未検出」タブ  
	--yahoo オプションを使用したときのみ作られる
	Twitter APIの検索でかからず、Yahoo!リアルタイム検索のみで得られるTweetを表示
* 「統計【フリクエ1部】」「統計【フリクエ1.5部】」「統計【フリクエ2部】」「統計【修練場】」タブ:  
それぞれをFGOアイテム効率劇場の楽屋風の統計データにしたもの
* 「Error」タブ:  
 FGO周回カウンタの書式に則らないお行儀の悪いツイートが収納される

# 注意
実行して作成されるファイルsetting.ini は絶対他人に渡してはいけない。Twitter アカウントの乗っ取りが可能になる。

# 制限
* デフォルトでは100件の検索を5回行った結果を表示しているが、リツイートは除外しているのと、そもそも検索結果が100件返ってくるわけではないので500件のデータにはならない
* Twitter API の制限によりsetting.ini のmaxloopをどんなに大きくしても10日前までのツイートしか検索できない。
* Twitter API の制限により短時間に大量に検索をかけると.RateLimitErrorエラーとなる。その場合15分待つこと。
* 今後追加される新しいフリクエには自動対応されるわけではない。freequest.csvやsyurenquest.csv やitem.csvを書き換えることで比較的容易に対応可能
* Twitter APIの仕様により、すべてのツイートが検索できるわけではない。~~Yahooリアルタイム検索機能を使えば検索できないツイートをほとんど拾えるが完璧に拾えるわけではない~~
* --checkreply オプション使用時に履歴にあるツイートが削除されたときツイートの復元はできない。単に無視される
* --checkreplyオプション使用時に元のツイートが「未検索」データでも、復元ツイートになると「未検索」という情報は落ちてしまう

# バグ
* UnicodeEncodeError がでる場合は、環境変数LC_CTYPEにja_jp.utf8 などのUTF-8を扱えるものをセットすること
* ~~Yahoo!リアルタイム検索ではすべてのデータが確実にとれるわけではなく、実行するたびにデータが間引かれたりすることがある~~

# 仕様
* TWITTER API を使用して #FGO周回タグカウンタ のハッシュタグを500件検索している(リツイートは除く)。
* 周回カウンタサイトを使用して作られた報告はユーザー定義以外全てエラーなく扱われる。
* 複数の報告を1ツイートで行ったものはエラータブ行きとなる。
* 常設フリークエスト・周蓮場のクエストのアイテムの報告順番はランダムでも受け付けられる。Excelファイル内では最終的にクエストのcsvの指定順に統一される。
* 常設のフリクエ・曜日クエストに関してはそのクエストにそのドロップアイテムが存在するかのチェックを行い、存在しない場合その報告はエラータブ行きとなる。
***
* 種火・スキル石以外の素材はドロップ率が規定より大きすぎるとエラータブ行きになる
1. 金素材：
* 周回数に関わらず泥率100%より大きい報告はエラー  
* 周回数100周以上の場合は泥率50%以上でエラー  
2. 銀素材：
* 周回数に関わらず泥率200%以上でエラー
* 周回数100周以上の場合は泥率70%以上でエラー
3. 銅素材：
* 周回数に関わらず泥率300%以上でエラー
* 周回数100周以上の場合は泥率90%以上でエラー
***

* 新章追加などでフリクエ・アイテムが追加された場合、csvファイルを編集して新規追加することでプログラム自体を修正することなく対応可能
* setting.ini のng_name にscreen_nameを記述することで指定ユーザーのツイートを集計から除外することが可能。複数記述する場合は半角スペースで開けて記述すること。

# コメントについて
次の例1のような、【クエスト名:】n周 と #FGO周回カウンタの外側にあるコメント1、コメント2はエラータブ行きにならずに扱われる

例1:

    (コメント1)
    【ユガ・クシェートラ 南の町】100周
    矢尻20-塵30-鎖30
    狂魔5-狂輝40
    #FGO周回カウンタ aoshirobo.net/fatego/rc/
    (コメント2)`

次の例2 のような内部にコメントが入っている場合、コメントがアイテム表記かが評価される。行末が数字かどうかで判断するので、「目標矢尻200個です」というコメントは問題にならないが、「目標矢尻200」だと「目標矢尻」というアイテムが200個ドロップした報告と判断される。常設フリクエの場合、そういったアイテムはないのでエラータブ行きになる。常設フリクエでない場合はその他のクエストタブ行きになる。コメントは【の前かURLの後に書くことを推奨する。
例2: 

    【ユガ・クシェートラ 南の町】100周
    矢尻20-塵30-鎖30
    狂魔5-狂輝40
    (コメント3)
    #FGO周回カウンタ aoshirobo.net/fatego/rc/

# 独自表記について

例3にあるような追加n周という報告はエラーなく扱われる。また、()は無視されるので(20%)といった表記があってもエラーにならない。このプログラムでは許容はしているが、そういった独自表記は【の前か、URLの後に書くことを推奨する。

例3:

    【ユガ・クシェートラ 南の町】追加100周
    矢尻20(20%)-塵30(30%)-鎖30(30%)
    狂魔5-狂輝40
    #FGO周回カウンタ aoshirobo.net/fatego/rc/

次の例4のようにアイテムを正式名称で書いてもエラーにならずに扱われる。しかしながら周回カウンタサイトで周回報告を作成することを推奨する。

例4:

    【ユガ・クシェートラ 南の町】100周
    禍罪の矢尻20-虚影の塵30-愚者の鎖30
    狂の魔石5-狂の輝石40
    #FGO周回カウンタ aoshirobo.net/fatego/rc/

その他
ハッシュタグを付け忘れたからといって例5のようにツイートにコメントでハッシュタグだけをつけてもTWITTER APIでの検索にかからないのでこのプログラムでは取得できない(=Twitterの流儀ともあわない)。元のコメント内容を含めて再ツイートすべきケースとなる。


例5:

    【ユガ・クシェートラ 南の町】100周
    禍罪の矢尻20-虚影の塵30-愚者の鎖30
    狂の魔石5-狂の輝石40

↓

    #FGO周回カウンタ

# 個人情報について
このプログラムでは、個人情報は一切集めていない。Twitterのアプリ認証は、Twitter APIによる検索と –nofavoritedを使用する場合はお気に入りリストの取得を使用するために必要なので行っている。 

