# Oh My Posh 導入方法

## インストール

1. まずは PowerShell を管理者権限で開き、以下を実行。
   最新版の PowerShell をインストールする。
   ```powershell
   winget install --id Microsoft.Powershell --source winget
   ```
1. PowerShell7 を管理者権限で開き、以下を実行。
   Oh My Posh をインストールする。
   ```powershell
   winget install JanDeDobbeleer.OhMyPosh -s winget
   ```
1. ここで PowerShell を再起動する。
1. フォントもインストールする。Oh My Posh のアイコンをしっかり表示させるには Nerd Font が必要。
   ```powershell
   oh-my-posh font install
   ```
   > ✅Point
   >
   > フォントの選択画面が出るので、好きなものを選ぶ！FiraCode がおすすめ！
1. PowerShell プロファイルを作成！
   ```powershell
   New-Item -Path $PROFILE -Type File -Force
   notepad $PROFILE
   ```
   メモ帳が開くので以下をコピペ
   ```powershell
   oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\jandedobbeleer.omp.json" | Invoke-Expression
   ```
   保存して閉じる
1. PowerShell を再起動！設定を反映させるために開きなおす！
1. かわいいプロンプトのできあがり！

## テーマの変え方

1. [Oh My Posh 公式サイト テーマギャラリー](https://ohmyposh.dev/docs/themes 'Oh My Posh公式サイト')にアクセス
1. お気に入りを見つけたら、テーマの名前をコピー
1. 先ほどのメモ帳の`jandedobbeleer`の部分をテーマの名前に変える

   (例：atomic にしたい場合)

   ```powershell
   oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\atomic.omp.json" | Invoke-Expression
   ```

1. 保存して、PowerShell を再起動
