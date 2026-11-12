# CLAUDE.md

ゲームの仕様やファイル構成は README.md にまとめています。

## デプロイを頼まれたとき

「デプロイして」「公開して」「本番に反映して」などと言われたら、確認を挟まずに次の手順で進めてください。

1. 未コミットの変更があれば、何が公開されるかをユーザーに伝える（コミットするかどうかはユーザーに確認する）
2. リポジトリ直下で `npx -y wrangler@4 deploy` を実行する
   - 公開先は Worker `abura-atsume`（https://abura-atsume.kiraides.workers.dev/）。設定は `wrangler.jsonc`
   - アップロードされたファイルの一覧に `index.html` と `vendor/liquidfun/` 以外が含まれていないか確認する
3. ログインが切れていて失敗した場合は、ユーザーにターミナルで `npx wrangler login` を実行してもらい、ブラウザでの許可を待ってから 2 をやり直す。OAuth の許可はユーザー本人が行うもので、こちらでは操作しない
4. 反映を確認する
   - `curl -s "https://abura-atsume.kiraides.workers.dev/?v=$(date +%s)" | cmp - index.html` で公開中の HTML がローカルと一致するか見る
   - `README.md` や `.git/config` が 404 になっているか見る
   - 内蔵ブラウザで公開URLを開き、「はじめる」でゲームが始まり、コンソールにエラーが出ないことを確かめる
5. 公開URLと、確認した内容をユーザーに報告する

GitHub への push では公開サイトは更新されない。push とデプロイは別の作業として扱う。
