# 環境
- Ruby 2.3.1 (by rbenv)
- MySQL 5.5
- Nginx

# 事前準備
- Amazon Linux であること
- ec2-user でログインできること

# カスタマイズ
- production の ip アドレスを変更する
- nginx.conf の templates を変更する

# 実行方法

    $ cd ansible-playbook
    $ ansible-playbook -i production production.yml --private-key="~/.ssh/priv_key.pem"
    $ ssh ec2-user@xxx.xxx.xxx.xxx && sudo mysql_install_db --datadir=/var/lib/mysql --user=mysql # mysql の起動で失敗した場合のみ

# 手作業
- `$ mysql_secure_installation` の実行
- ユーザーの作成と鍵の登録
