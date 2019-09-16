# DMOJのセットアップ
Ubuntu 18.04でDMOJをインストールした際の手順を書きました。基本的には下の２つのサイト（ドキュメント）がベースとなっています。

- [DMOJ Docs](https://dmoj.readthedocs.io/en/latest/)
- [DMOJ site-docker](https://github.com/DMOJ/site-docker)

上記のサイトでは、Python2で構築していますが、最新のソースコードでは、Python2はサポートされていません。  
また、日本語環境では、文字コードの違いにより、Mariadbではエラーが出るため、代わりにMySQLを使用しています。

## INDEX
- [Siteのセットアップ](https://github.com/kitakaze0804/DMOJ-Setting/site-setup.md)
- [Siteのアップデート（デザインの変更など）](https://github.com/kitakaze0804/DMOJ-Setting/site-update.md)
- [Judgeのセットアップ](https://github.com/kitakaze0804/DMOJ-Setting/judge.md)
- [Site-Judgeの接続とサンプル問題の設定](https://github.com/kitakaze0804/DMOJ-Setting/dmoj-connection.md)
- [他のプログラミング言語の追加](https://github.com/kitakaze0804/DMOJ-Setting/otherlangage.md)