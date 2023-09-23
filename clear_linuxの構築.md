# 概要

wslへのclear linux導入手順です。

# WSL2にいれる

on powershell

```ps1
docker pull clearlinux
docker run clearlinux
docker container ls -a
docker export --output="clearlinux.tar" hogehoge
wsl --import Clear "C:\wsl\clear" .\clearlinux.tar
```

## ユーザー追加

## rootで以下の作業

on clear

```sh
useradd your_user_name
passwd your_user_name

usermod -G wheel -a your_user_name
cat << EOF > /etc/wsl.conf
[user]
default=your_user_name
EOF
swupd bundle-add sudo
exit
```

## wsl再起動

on powershell

```ps1
wsl --shutdown
```

## ログインし直して以下

on clear

```sh
sudo swupd update
exit
```

## ここまでの環境を保存

on powershell

```ps1
wsl -t Clear
wsl -l -v
wsl --export Clear ./clearlinux.tar
```

# 個人的な環境整備

on clear

```sh
# brew
sudo swupd bundle-add git curl
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/your_user_name/.profile
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

# install
brew install zsh git gh tmux neovim juliaup

# setting
zsh
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"

mkdir git;cd git;gh auth login

gh repo clone izumi4632/dotfiles;
cd ./dotfiles;_ chmod +x ./*;./make_symbolic_link.sh;./setting_zsh.sh

# change shell
command -v zsh | sudo tee -a /etc/shells
sudo cat << EOF > /etc/passwd
root:x:0:0:root:/root:/bin/bash
your_user_name:x:1000:1000::/home/your_user_name:/home/linuxbrew/.linuxbrew/bin/zsh
EOF
```
