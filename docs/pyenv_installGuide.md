# pyenv install guide

## 1. Install pyenv

`sudo apt update`, `sudo apt upgrade` 後、以下のコマンドを実行してください。
※Gitから必要なパッケージをインストールします。

```bash
sudo apt update
sudo apt upgrade
sudo apt install -y build-essential libssl-dev libssl-dev zlib1g-dev liblzma-dev libbz2-dev \
libreadline-dev libsqlite3-dev libopencv-dev tk-dev git
sudo apt install -y libffi-dev
```

次にgitから、pyenvをcloneします。

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

## 2. bashrcと.profileにパスを追加

```bash
# .bashrcに以下を追記 一般のubuntuの場合
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export command -v pyenv > /dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc

# WSL2のubuntuの場合
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

追加できたら

```bash
source ~/.bashrc
```

pyenvのバージョン確認

```bash
pyenv --version
```

## 3. pyenvでpythonをインストール

```bash
# インストール可能なpythonのバージョンを確認
pyenv install -list

# 希望のバージョンをインストール
python install 3.11.4

# インストールできたら、確認
pyenv versions

# pyenvでglobalに設定
pyenv global 3.11.4
```

これでpythonのバージョンの確認ができる

```bash
python -V
```
