# with_methodsに関するコードリーディング

## コード
``` rs
pub(crate) fn with_methods(input: TokenStream) -> TokenStream {
    let mut ast = match syn::parse::<syn::ItemFn>(input.clone()) {
        Ok(ast) => ast,
        // on parse error, make IDEs happy; see fn docs
        Err(err) => return input_and_compile_error(input, err),
    };

    let (methods, others) = ast
        .attrs
        .into_iter()
        .map(|attr| match MethodType::from_path(attr.path()) {
            Ok(method) => Ok((method, attr)),
            Err(_) => Err(attr),
        })
        .partition::<Vec<_>, _>(Result::is_ok);

    ast.attrs = others.into_iter().map(Result::unwrap_err).collect();

    let methods = match methods
        .into_iter()
        .map(Result::unwrap)
        .map(|(method, attr)| {
            attr.parse_args()
                .and_then(|args| Args::new(args, Some(method)))
        })
        .collect::<Result<Vec<_>, _>>()
    {
        Ok(methods) if methods.is_empty() => {
            return input_and_compile_error(
                input,
                syn::Error::new(
                    Span::call_site(),
                    "The #[routes] macro requires at least one `#[<method>(..)]` attribute.",
                ),
            )
        }
        Ok(methods) => methods,
        Err(err) => return input_and_compile_error(input, err),
    };

    match Route::multiple(methods, ast) {
        Ok(route) => route.into_token_stream().into(),
        // on macro related error, make IDEs happy; see fn docs
        Err(err) => input_and_compile_error(input, err),
    }
}
```

## リーディング

### 引数解析

```rs
 let mut ast = match syn::parse::<syn::ItemFn>(input.clone()) {
        Ok(ast) => ast,
        // on parse error, make IDEs happy; see fn docs
        Err(err) => return input_and_compile_error(input, err),
    };
```
- 引数にはinput(TokenStream)を受け取る
- `syn`パッケージのparseメソッドで引数を解析(返り値はResult<>)
- OKなら...

### 題材2

```rs
  let (methods, others) = ast
        .attrs
        .into_iter()
        .map(|attr| match MethodType::from_path(attr.path()) {
            Ok(method) => Ok((method, attr)),
            Err(_) => Err(attr),
        })
        .partition::<Vec<_>, _>(Result::is_ok);
```

- ...
- ...
- [MethodType](./MethodType.md)でマッチ
