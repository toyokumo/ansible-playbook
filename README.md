# Ansible Playbook

## Services
- `Ruby 2.4.1 (by rbenv)`
- `PHP 7.1`
- `Java9`
- `MySQL 5.7`
- `PostgreSQL`
- `Nginx + Unicorn`
- `Postfix`
- `rkhunter`
- `Redis`

## Prepare
- Amazon Linux であること
- ec2-user で ssh ログインできること

## Setup
Ansible の実行に踏み台サーバーを利用したい場合 `ssh_config` ファイルを生成する

### Shared Setup
- develop or production の ip アドレスを変更
- `group_vars/all` の停止サービスを確認
- `group_vars/all` に必要なユーザーを追記、パスワードは `openssl passwd -1 your-password` で生成
- `roles/common/files/authorized_keys_for_username` の `user_name` 部分をリネームする、必要ユーザーの数だけファイル作成し公開鍵を追加
- 必要であれば `roles/rkhunter/vars/main.yml` にメールアドレス記載

### Nginx Setup
- `roles/nginx/vars/main.yml` を変更

### MySQL Setup
- `roles/mysql/vars/main.yml.example` を `roles/mysql/vars/main.yml` にリネームして適宜修正

### PostgreSQL Setup
- `roles/postgresql/vars/main.yml` にある環境変数をセットする

### Postfix Setup
- `roles/postfix/files/main.cf` を適宜修正

以下必要最低限の変更

```
myhostname = set-your-hostname
mydomain = set-your-domain
```

## Usage

### All in One Environment

一つのサーバーに common, web, db, php, ruby, postfix を全て含ませるパターン

    $ ansible-playbook -i <hosts> all_in_one.yml

### Independent Environment

役割ごとに構築するパターン

    # common & web
    $ ansible-playbook -i <hosts> web.yml

    # common & db
    # MySQL か PostgreSQL の使う方をコメントアウトする
    $ ansible-playbook -i <hosts> db.yml

### PHP Application

    # common & web & php
    $ ansible-playbook -i <hosts> php.yml

### Ruby on Rails Application

    # common & web & ruby
    $ ansible-playbook -i <hosts> ruby.yml

### Java Application

    $ ansible-playbook -i <hosts> java.yml

### Middlewara if you needed

    $ ansible-playbook -i <hosts> rkhunter.yml
    $ ansible-playbook -i <hosts> postfix.yml
    $ ansible-playbook -i <hosts> redis.yml

### Tips

Syntax Check

    $ ansible-playbook -i <hosts> <playbook.yml> --syntax-check

Task List

    $ ansible-playbook -i <hosts> <playbook.yml> --list-tasks

Dry Run

    $ ansible-playbook -i <hosts> <playbook.yml> --private-key="~/.ssh/priv_key.pem" --check

## Error Tips

    # mysql の起動で失敗した場合
    $ ssh ec2-user@xxx.xxx.xxx.xxx && sudo mysql_install_db --datadir=/var/lib/mysql --user=mysql
