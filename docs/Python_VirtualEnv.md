# Pythonの仮想環境を作成する

## 1. 仮想環境を作成

```bash
python -m venv .venv
```

このときのvenvは、pythonのバージョンに合わせて作成されます。

## 2. 仮想環境を有効化

```bash
source .venv/bin/activate
```

これで有効になります。
※有効化してから、pip install で必要なパッケージをインストールします。

## 3. 仮想環境を無効化

```bash
deactivate
```