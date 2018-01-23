# Ansible Playbook

## Services
- `Ruby 2.4.1 (by rbenv)`
- `PHP 7.1`
- `Java9`
- `MySQL 5.7`
- `PostgreSQL`
- `Nginx`
- `Postfix`
- `rkhunter`
- `Redis`

## Prepare
- Amazon Linux であること
- ec2-user で ssh ログインできること

## Setup
Ansible の実行に踏み台サーバーを利用したい場合 `ssh_config` ファイルを生成する

### Shared Setup
- develop, staging, production の ip アドレスを変更
- `group_vars/all` の停止サービスを確認
- `group_vars/all` に必要なユーザーを追記、パスワードは `openssl passwd -1 your-password` で生成
- `roles/common/files/authorized_keys_for_username` の `user_name` 部分をリネームする、必要ユーザーの数だけファイル作成し公開鍵を追加
- `roles/common/vars/main.yml` で SSH ログインを許可する from ip アドレスを記述
- 必要であれば `roles/rkhunter/vars/main.yml` にメールアドレス記載

### Nginx Setup
- `vars/` 配下のファイルを確認変更

### MySQL Setup
- `roles/mysql/vars/main.yml.example` を `roles/mysql/vars/main.yml` にリネームして適宜修正
- `db.yml` を修正

### PostgreSQL Setup
- `roles/postgresql-server/vars/main.yml.example` を `roles/postgresql-server/vars/main.yml` にリネームして適宜修正
- `db.yml` を修正

### Postfix Setup
- `vars/` 配下のファイルを確認変更

## Usage

```
$ ansible-playbook -i <hosts> nginx.yml
$ ansible-playbook -i <hosts> db.yml # MySQL か PostgreSQL の使う方をコメントアウトする
$ ansible-playbook -i <hosts> php.yml
$ ansible-playbook -i <hosts> ruby.yml
$ ansible-playbook -i <hosts> java.yml
$ ansible-playbook -i <hosts> rkhunter.yml
$ ansible-playbook -i <hosts> postfix.yml
$ ansible-playbook -i <hosts> redis.yml
```

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
