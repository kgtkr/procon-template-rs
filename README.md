# Rustの競プロ用テンプレート

## 使い方

プロジェクトを作成してテンプレートをmain.rsに貼り付けます。
```
$ cargo new 問題名
$ cd 問題名
$ curl https://raw.githubusercontent.com/kgtkr/procon-template-rs/master/src/main.rs > src/main.rs
```

main.rsを編集します
`cargo test`でユニットテストをします
提出します

## solve関数
ここに処理を書きます。
標準入力をStringとして受け取り、標準出力として出力する内容をStringとして返します。
標準入力のパースにはinputマクロを使うと便利です。

## testsマクロ
ユニットテストを行います。

```rs
tests! {
    テスト名: "入力" => "期待される結果",
}
```

という構文です。
AtCoder(beta版)の場合はユーザースクリプトを使う事でサンプルケースの自動生成を行う事が出来ます。
https://github.com/kgtkr/atcoder-create-test

## inputマクロ
入力値をパースするマクロです。

```rs
input!(入力値=>形式定義);
```

のように使います。
例えば

```rs
input!("1 2"=>(a:i64 b:i64));
```

のようにするとaには1が、bには2が束縛されます。

### 形式定義の構文
input!(input=>行パーサー...);

#### 行パーサー
(変数名:値パーサー...)もしくは{行数;変数名:値パーサー}と書きます。
後者は複数行の入力がある時に使います。

#### 値パーサー

|構文|解説|例|例入力|
|:-:|:-:|:-:|:-:|
|型|単一の型とマッチ(`parse::<型>()`でパースされる)|i64|10|
|#|文字列|#|abc|
|@|1から始まるインデックスを0からに変換するやつ。<br>つまりusizeでパースして-1してるだけ|@|1|
|[型]|配列|[i64]|1 2 3 4 5|
|(型,...)|タプル|(i64,#)|10 abc|

### 例
```rs
    input!(
    "3
5 2
2 3 4 5 6
10
20
30
1 2
8 1
2 3
4 1
1283 23 43 32
1 2 3
2 3 4
3 4 5
10 20 30
abc aaa
"=>
    (n:usize)
    (k:i64 p:i64)
    (list1:[i64])
    {n;list2:i64}
    (tup:(i64,i64))
    {n;list3:(i64,i64)}
    (i:i64 list4:[i64])
    {n;map:[i64]}
    (index:[@])
    (strs:[#])
  );
assert_eq!(n, 3);
assert_eq!(k, 5);
assert_eq!(p, 2);
assert_eq!(list1, vec![2, 3, 4, 5, 6]);
assert_eq!(list2, vec![10, 20, 30]);
assert_eq!(tup, (1, 2));
assert_eq!(list3, vec![(8, 1), (2, 3), (4, 1)]);
assert_eq!(i, 1283);
assert_eq!(list4, vec![23, 43, 32]);
assert_eq!(map, vec![vec![1, 2, 3], vec![2, 3, 4], vec![3, 4, 5]]);
assert_eq!(index, vec![9, 19, 29]);
assert_eq!(strs, vec!["abc", "aaa"]);
```