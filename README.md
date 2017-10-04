# Ansible Playbook

## Services
- `Ruby 2.4.1 (by rbenv)` or `PHP 7.1`
- `MySQL 5.7`
- `Nginx + Unicorn`
- `Postfix`
- `rkhunter`

## Prepare
- Amazon Linux であること
- ec2-user で ssh ログインできること

## Setup

### Shared Setup
- develop or production の ip アドレスを変更
- `group_vars/all` の停止サービスを確認
- `group_vars/all` に必要なユーザーを追記、パスワードは `openssl passwd -1 your-password` で生成
- `roles/common/files/authorized_keys_for_username` に公開鍵を追加
- 必要であれば `roles/rkhunter/vars/main.yml` にメールアドレス記載

### Nginx Setup
- `roles/nginx/vars/main.yml` を変更

### MySQL Setup
- `roles/mysql/vars/main.yml.example` を `roles/mysql/vars/main.yml` にリネームして適宜修正

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

    # develop
    $ ansible-playbook -i develop all_in_one.yml --private-key="~/.ssh/priv_key.pem"

    # production
    $ ansible-playbook -i production all_in_one.yml --private-key="~/.ssh/priv_key.pem"

### Independent Environment

役割ごとに構築するパターン

    # common & web
    $ ansible-playbook -i production web.yml --private-key="~/.ssh/priv_key.pem"

    # common & db
    $ ansible-playbook -i production db.yml --private-key="~/.ssh/priv_key.pem"

### PHP Application

    # common & web & php
    $ ansible-playbook -i production php.yml --private-key="~/.ssh/priv_key.pem"

### Ruby on Rails Application

    # common & web & ruby
    $ ansible-playbook -i production ruby.yml --private-key="~/.ssh/priv_key.pem"

### Middlewara if you needed

    $ ansible-playbook -i production rkhunter.yml --private-key="~/.ssh/priv_key.pem"
    $ ansible-playbook -i production postfix.yml --private-key="~/.ssh/priv_key.pem"

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
