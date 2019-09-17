# SiteとJudgeの接続方法
ここでは、SiteとJudgeを通信可能にする方法（Judgeを動かす方法）について説明します。
## 問題を格納するフォルダの作成
```
mkdir ~/dmoj/problems
```
## judge.ymlのダウンロードと修正
[judge.yml](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/judge.yml)を`~dmoj/judge`にダウンロードします。  
４行目の`kitakaze`を自分のユーザー名に変更します。
```
3: problem_storage_root: 
4:   - /home/kitakaze/dmoj/problems
```
## 設定ファイルの修正とコピー
[dmoj.conf](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/setting-files/dmoj.conf)を`~dmoj/judge`直下に置きます。  
2行目にある`kitakaze`を自分のユーザー名に変更します。
```
2: command=dmoj -c /home/kitakaze/dmoj/judge/judge.yml 127.0.0.1
```
Supervisorにコピーして再起動します。
```
sudo cp dmoj.conf /etc/supervisor/conf.d/
sudo supervisorctl update
sudo supervisorctl status
```
## サンプル問題のダウンロード
```
cd ~/dmoj
git clone https://github.com/DMOJ/docs.git
```

## Judgeに接続
webブラウザで http://localhost/admin を開きます。  
画面真ん中のだいぶ下にあるJudgeをクリックします。  
![image1](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection1.png)  

右上のAddを選択します。  
![image2](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection2.png)  

judge.ymlに書かれているIDとパスワードを入力します。  
- ID:ExampleJudge
- パスワード：keyのところに書かれている長い文字列

![image3](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection3.png)  

かなり下のほうにある`Save`ボタンを押して、登録します。  
30秒から1分ほど経ってから再読み込みすると、チェックマークがついて、使えるようになります。

## 問題のテストケースを登録
http://localhost に移動して、上のメニューから`problem`を選択します。  
一覧にある中から、aplubをクリックします。  
右側にある`Edit test data`を選びます。  
![image4](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection4.png)  

`Data zip File`の参照ボタンをクリックします。  
![image5](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection5.png)  

次の場所にある`aplusb.zip`を選択します。 
 ~/dmoj/docs/problem_examples/standard/aplusb  
![image6](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection6.png)  

ファイルを選択した状態で一度、`Submit`ボタンを押します。そのあとは、写真の通りにテストケースを設定します。右下にある`add new case`をクリックすると、テストケースを追加することができます。  
![image7](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection7.png)  

再びSubmitをして問題ページに戻ると、Judgeのところに`ExampleJudge`と表示されるはずです。  
![image8](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection8.png)  

## 動作確認
問題を提出して、採点ができていれば正常に動作しています。
![image9](https://raw.githubusercontent.com/kitakaze0804/DMOJ-Setting/master/images/judge_connection9.png)  
