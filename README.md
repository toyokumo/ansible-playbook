# 環境
- Ruby
- MySQL
- Nginx

# 事前準備
- Amazon Linux であること
- ec2-user でログインできること

# 実行方法

    $ cd ansible-playbook
    $ ansible-playbook -i production production.yml
    $ ssh ec2-user@xxx.xxx.xxx.xxx && sudo mysql_install_db --datadir=/var/lib/mysql --user=mysql # mysql の起動で失敗した場合のみ

# カスタマイズ
nginx.conf の server_name を変更する
