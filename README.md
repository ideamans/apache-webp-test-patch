# .htaccessによるWebP対応のパッチテスト

WebサーバーにApacheをお使いのサイトで、`.haccess`ファイルによるWebP画像の振り分けを行うためにはいくつかのモジュールが必要です。

* mod_rewrite
* mod_setenvif

これらの稼働状況を確認するテストパッチです。

# 使い方

1. このプロジェクトに含まれる`webp-patch`ディレクトリを、Webサーバーの任意のディレクトリに設置してください。 例 `/webp-patch`
2. ブラウザで設置したディレクトリを表示してください。例 `http://example.com/webp-patch/`
3. 期待動作は、対応ブラウザでは`WebP`、非対応ブラウザでは`PNG`と表示されることです。

期待動作を示した場合、そのWebサーバーでは追加設定不要で`.hatccess`ファイルによるWebP画像の振り分けが可能である可能性が高いです。

`.htaccess`ファイルを適用範囲のディレクトリに設置(既存の`.htaccess`がある場合はマージ)してください。

# WebP画像のファイル名規約

WebP画像のファイル名は、元になった画像ファイルの拡張子の後に`.webp`が付与されている前提です。

例えば`photo.jpg`のWebP画像は`photo.jpg.webp`です。

# トラブルシューティング

## Internal Server Errorになる場合

Webサーバーで`mod_rewrite`または`mod_setenvif`がインストールされていません。

`.htaccess`ファイルの次の記述はCDN向けの設定です。不要な場合は削除してお試しください。

```
SetEnvIf Request_URI "\.(jpe?g|png)$" _image_request
Header append Vary Accept env=_image_request
```

## WebP対応ブラウザでも`PNG`が表示される場合

`.htaccess`自体が機能していません。

* `AllowOverride`が`All`や`FileInfo`になっていない
* WebサーバーがApacheではない