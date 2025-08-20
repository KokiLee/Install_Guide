# SSH 鍵認証接続手順（Windows → Ubuntu）

## 1. 秘密鍵・公開鍵の作成（Windows 側）

1. PowerShell で鍵を作成：
```powershell
ssh-keygen -t ed25519 -C "koki@myPC"
```
- `-t ed25519` … 安全で高速な鍵アルゴリズム
- `-C "コメント"` … 鍵のラベル（好きな文字でOK）
- デフォルトでは `C:\Users\koki\.ssh\id_ed25519` に保存される

---

## 2. サーバ側の準備（Ubuntu）

1. `.ssh` ディレクトリ作成：
```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

2. 公開鍵をサーバに登録：

### 方法A: scp でコピー
```powershell
scp -P 50100 C:\Users\koki\.ssh\id_ed25519.pub koki@172.21.249.93:~/.ssh/temp.pub
```
サーバ側で：
```bash
cat ~/.ssh/temp.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
rm ~/.ssh/temp.pub
```

### 方法B: 手動コピー
1. Windows の `id_ed25519.pub` を開き内容をコピー  
2. サーバで `nano ~/.ssh/authorized_keys` を作成し貼り付け  
3. 保存後に権限を設定：
```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## 3. サーバの sshd 設定確認

1. 設定ファイルを開く：
```bash
sudo nano /etc/ssh/sshd_config
```

2. 以下を確認・設定：
```conf
PubkeyAuthentication yes       # 鍵認証を有効化
PasswordAuthentication no      # パスワードログインを禁止（任意）
PermitRootLogin no              # rootログイン禁止（推奨）
Port 50100                      # SSH 待ち受けポート
```

3. 変更後に再起動：
```bash
sudo systemctl restart ssh.service
```

---

## 4. Windows からの接続テスト

```powershell
ssh -i C:\Users\koki\.ssh\id_ed25519 -p 50100 koki@172.21.249.93
```

- `-i` … 秘密鍵を明示的に指定  
- `-p 50100` … SSH サーバが待ち受けるポート

---

## 5. VS Code Remote-SSH で接続

1. SSH 設定ファイル（`~/.ssh/config`）に追加：
```sshconfig
Host RYZEN-PC
    HostName 172.21.249.93
    User koki
    Port 50100
    IdentityFile C:\Users\koki\.ssh\id_ed25519
```

2. VS Code の Remote-SSH で `RYZEN-PC` を選択して接続  

---

## 6. 注意点

- `.ssh` ディレクトリ → `700`、`authorized_keys` → `600` に権限を設定する  
- 秘密鍵は絶対に他人に渡さない  
- 公開鍵認証ができたら、必要に応じて `PasswordAuthentication no` に変更するとセキュア

---

💡 **ポイントまとめ**  
- 鍵作成（Windows） → 公開鍵登録（Ubuntu） → sshd 設定確認 → 接続テスト → VS Code 設定  
- ポート番号、鍵パス、権限が正しいことが接続成功の鍵

