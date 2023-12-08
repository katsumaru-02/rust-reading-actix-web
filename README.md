# actix-webのコードリーディング

## ディレクトリ構成

#### actix-web

Actix Web本体

#### actix-web-codegen

Actix Webのルーティングとランタイムマクロ
> Actix Webはこのクレートのバージョン全体を再エクスポートするので、通常はこのクレートへの依存関係を明示的に指定する必要はありません。しかし、actix-web依存関係が更新される前にこのクレートに更新が行われることもあります。そのため、ここでのコード例では明示的なインポートを示します。どのマクロが再エクスポートされるかは、最新のactix-web属性ドキュメントを確認してください。

- [ドキュメント](https://docs.rs/actix-web-codegen/latest/actix_web_codegen/)

#### actix-files

## 依存クレート

#### proc_macro

> マクロ作者が新しいマクロを定義する際のサポートライブラリです。
> このライブラリは標準ディストリビューションで提供され、関数型マクロ#\[proc_macro\]、マクロ属性#\[proc_macro_attribute\]、カスタム派生属性#\[proc_macro_derive\]などの手続き的に定義されたマクロ定義のインタフェースで使用される型を提供します。

- [コードリーディング](https://github.com/katsumaru-02/rust-reading-proc_macro)
- [ドキュメント](https://doc.rust-lang.org/proc_macro/index.html)

## 参考文献

- [ドキュメント](https://docs.rs/actix-web/latest/actix_web/)
