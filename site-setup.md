# Siteのセットアップ
## 必要なパッケージのダウンロード
必要なソフトウェアをすべてダウンロードします。
```
sudo apt install git gcc g++ make python-dev libxml2-dev libxslt1-dev zlib1g-dev gettext curl python3-pip mysql-server libmysqlclient-dev supervisor nginx
```
## Nodejsのダウンロードとインストール
ここでは、バージョン８を使用します。ほかのバージョンでも動くかもしれません。また、`npm`で追加で必要なパッケージもインストールしてしまいます。
```
wget -O- https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt install -y nodejs
sudo npm install -g --unsafe-perm=true sass pleeease-cli
```

## MySQLの設定
必ずスーパーユーザーとして実行してください。
```
sudo mysql_secure_installation
``` 
起動すると、
> Would you like to setup VALIDATE PASSWORD plugin?

と聞かれるので、nキーを押して拒否します。 

新しいパスワードの入力を求められるので、パスワードを決め、入力する。（ここでは、`dmoj`としました。）  
再度パスワードを入力し、エンターを押す。
> Remove anonymous users?  
Disallow root login remotely?  
Remove test database and access to it?  
Reload privilege tables now?  

上記の質問には、yキーで許可する。

## データベースの設定
まずは、下記のコマンドで、MySQLサーバにログインする。（先ほど設定したパスワードを入力）
```
sudo mysql -u root -p
```
次に２つのコマンドで、dmojのデータベースを登録し、終了する。（セミコロンで分けて入力）
```
CREATE DATABASE dmoj DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_general_ci;
GRANT ALL PRIVILEGES ON dmoj.* to 'dmoj'@'localhost' IDENTIFIED BY '<password>';
exit
```

## Siteのソースコードをダウンロード
ホームディレクトリ直下に`dmoj`というフォルダを作り、その下で作業します。
```
mkdir ~/dmoj
cd ~/dmoj
git clone https://github.com/DMOJ/site.git
cd site/
git submodule init
git submodule update
```

## 設定ファイルについて
以下のファイルをダウンロードして、`~/dmoj/site`直下に保存してください。
- [site.conf](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/site.conf)
- [bridged.conf](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/bridged.conf)
- [uwsgi.ini](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/uwsgi.ini)
- [ngix.conf](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/ngix.conf)
- [config.js](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/config.js)
- [wsevent.conf](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/wsevent.conf)

また、[localsetting.py](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/localsetting.py)をsite内のdmojに配置し、ファイル内の`kitakaze`の部分を自分のユーザー名に変更してください。
```
49: DMOJ_PROBLEM_DATA_ROOT = '/home/kitakaze/dmoj/problems'
124: STATIC_ROOT = '/home/kitakaze/dmoj/site/static'
```
## モジュールのインストール
次はモジュールをインストールしていきます。必ず`sudo`で実行することと、`pip`ではなくて、`pip3`を使うようにしましょう。
```
sudo pip3 install -r requirements.txt
sudo pip3 install mysqlclient
python3 manage.py check
```
最後のコマンドは**sudoをつけずに**実行します。エラーが出なければ成功です。

## サイトのコンパイル
いよいよスタイルシートをコンパイルします。
```
./make_style.sh 
python3 manage.py collectstatic
python3 manage.py compilemessages
python3 manage.py compilejsi18n
```

## データベーステーブルのセットアップ
先ほど設定したデータベースに書き込みをします。
```
python3 manage.py migrate
python3 manage.py loaddata navbar
python3 manage.py loaddata language_small
python3 manage.py loaddata demo
```

## 管理者ユーザの作成
管理者ユーザを作成します。メールアドレスは空のままで、結構です。
```
python3 manage.py createsuperuser
```

## uWSGIのセットアップ
uwsgiをインストールし、設定ファイルをコピーします。  
**site.conf,bridged.confの２・３行目にあるホームディレクトリのパスの"kitakaze"の部分を自分のユーザー名に書き換えてください。**  
**また、uwsgi.iniの12行目も同じく変更してください。**
```
sudo pip3 install uwsgi
sudo cp site.conf bridged.conf /etc/supervisor/conf.d/
```

## Supervisordの再起動
以下のコマンドで、supervisordを再起動し、正常に動作しているか確認します。  
```
sudo supervisorctl update
sudo supervisorctl status
```
statusがRUNNINGとなっていたら、正常です。

## ngixのセットアップ
ngix.confを次のコマンドで、`/etc/nginx/sites-enabled`に配置します。そのパスには`default`というファイルがあるため今回はファイル名を`default`として、上書きコピーしています。  
**ファイル内の21行目、25行目、40行目のパスの"kitakaze"の部分を自分のユーザー名に書き換えてください。**

```
sudo cp nginx.conf /etc/nginx/sites-enabled/default
sudo service nginx reload
```
ngixサービスを再起動して、エラーが出なければ成功です。

## イベントサーバーの構築
config.jsをwebsocketフォルダに配置し、必要なパッケージをインストールします。
```
cp config.js websocket/
sudo npm install qu ws simplesets
sudo pip3 install websocket-client
sudo pip3 install django_select2
```
また、wsevent.confをsupervisorにコピーして supervisordを再起動します。  
**wsevent.confの２・３行目のフォルダパスを直してください。**
```
sudo cp wsevent.conf /etc/supervisor/conf.d/
sudo supervisorctl reload
sudo supervisorctl status
```

## サイトの確認
webブラウザを開き、アドレスバーに`localhost`（127.0.0.1と同じ）と入力します。サイトが表示されれば成功！  
管理者ユーザでログインできることを確認してください。