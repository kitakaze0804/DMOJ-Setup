# Siteのアップデート
Siteは日々更新されているため、ソースコードを更新して、コンパイルしなおすことで、ローカル環境でも新しい環境にすることができます。
## ソースコードの更新
siteフォルダに移動して、次のコマンドで更新します。
```
cd ~/dmoj/site
git pull origin master
```
もし、requirements.txtに新しいモジュールが追加された場合は、次のコマンドでインストールしてください。
```
sudo pip3 install -r requirements.txt
```
## データベースの更新
スキーマが更新されている場合は、次の手順で更新します。
```
python3 manage.py migrate
python3 manage.py check
```

## staticファイルの更新
最後に、外観のデザインに関わるstaticファイルの更新方法を紹介します。
```
./make_style.sh
python3 manage.py collectstatic
python3 manage.py compilemessages
python3 manage.py compilejsi18n
```

## （補足）データベースの削除
データベースを一度完全に消したいときには次の手順で実行します。
### データベースサーバにログイン
```
sudo mysql -u root -p
```
### データーベースの削除
```
# データベースの確認
show databases;
# データベースの削除
drop database dmoj;
```
ただし、データベースごと消してしまったので、次のコマンドで、もう一度`dmoj`を作り直します。
```
CREATE DATABASE dmoj DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_general_ci;
```