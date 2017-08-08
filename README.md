# Ansible Playbook

## Services
- `Ruby 2.4.1 (by rbenv)` or `PHP 7.1`
- `MySQL`
- `Nginx + Unicorn`

## Prepare
- Amazon Linux であること
- ec2-user で ssh ログインできること

## Setup
- develop or production の ip アドレスを変更する
- nginx.conf の templates を変更する

## Usage

### All in One Environment

一つのサーバーに common, web, db, php, ruby を全て含ませるパターン

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

### Tips

Syntax Check

    $ ansible-playbook -i <hosts> <playbook.yml> --syntax-check

Task List

    $ ansible-playbook -i <hosts> <playbook.yml> --private-key="~/.ssh/priv_key.pem" --list-tasks

Dry Run

    $ ansible-playbook -i <hosts> <playbook.yml> --private-key="~/.ssh/priv_key.pem" --check

## Error Tips

    # mysql の起動で失敗した場合
    $ ssh ec2-user@xxx.xxx.xxx.xxx && sudo mysql_install_db --datadir=/var/lib/mysql --user=mysql

## TODO: Manual
- `$ mysql_secure_installation` の実行
- ユーザーの作成と鍵の登録
