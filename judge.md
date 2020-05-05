# Jugeのセットアップ
Judgeは比較的簡単にセットアップできます。  
（ホームディレクトリの直下に`dmoj`というフォルダがある前提で話を進めます）
## ソースコードのダウンロード
```
cd ~/dmoj
git clone https://github.com/DMOJ/judge-server.git judge
cd judge
git submodule init
git submodule update
```
## 必要なパッケージのインストール
libseccompがないと、ビルド時にヘッダーファイルのエラーが出ます。[☆](https://github.com/DMOJ/judge/issues/483)
```
sudo apt install -y libseccomp-dev
sudo pip3 install -r requirements.txt
```

## Judgeのビルド
```
sudo python3 setup.py develop
```
ビルドが終了したら、
```
dmoj --help
```
で使い方が表示されることを確認してください。