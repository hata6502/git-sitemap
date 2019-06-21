# git-sitemap

git 管理による静的サイトのサイトマップを作成します。

## 使い方

サイトマップを作成する git リポジトリにて `git sitemap` コマンドを実行します。
コマンドを実行したディレクトリ配下の `.html` ファイルを探索します。

(例)

```
$ git add .
$ git commit -m "記事の執筆者名を修正しました。"
$ git sitemap > sitemap.xml
$ cat sitemap.xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<url><loc>https://b-hood.site/articles/analog-orgel/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>daily</changefreq></url>
<url><loc>https://b-hood.site/articles/gitwitter/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/mvhash/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/pre_commit/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/sitemap/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/staticgen/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/sticky-stack/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/table-of-contents/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/trimrate/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/articles/webp/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/</loc><lastmod>2019-06-18T22:06:30+09:00</lastmod><changefreq>weekly</changefreq></url>
<url><loc>https://b-hood.site/privacy/</loc><lastmod>2019-06-18T00:16:13+09:00</lastmod><changefreq>monthly</changefreq></url>
</urlset>
```

## インストール

`/usr/local/bin` などに `git-sitemap` を配置し、実行権限を付与します。

## 設定

`.git-sitemaprc` を設置して設定をします。
リポジトリのルートディレクトリから探索し、各 `.html` ファイルのパスまで辿りながら
`.git-sitemaprc` に記述された設定をオーバーライドしていきます。

```
prefix="https://example.com"  # URL 前置詞
```

## サイトマップへの登録対象

git へのインデックスが作られている `.html` ファイルが対象となります。
`.gitignore` によって無視されたページは登録されません。

## changefreq の判定

changefreq は、`.html` ファイルのコミット履歴をもとに自動生成されます。
`.html` ファイルの更新判定を行い、更新回数と作成日時から changefreq を判定します。
更新の判定プログラムは、`.git-sitemaprc` に `difftest` 関数を記述することで設定できます。
デフォルトでは必ず「更新」と判定されます。

(例. 1) 10 行以上の変更で「更新」と判定する bash スクリプト

```
function difftest () {
  # $1 コミットのハッシュ
  # $2 ファイルパス

  local diff=`git diff -U0 $1^..$1 $2 | tail -n +5 | grep -E "^(\+|-)"`
  local count=`echo "${diff}" | wc -l`
  return `[ ${count} -ge 10 ]`
}
```

(例. 2) css ファイルのハッシュ値変化を無視する bash スクリプト

```
function difftest () {
  # $1 コミットのハッシュ
  # $2 ファイルパス

  local diff=`git diff -U0 $1^..$1 $2 | tail -n +5 | grep -E "^(\+|-)"`
  # /css/app.css?id=99a477ea...
  diff=`echo "${diff}" | grep -v "\/css\/app.css"`
  return `[ -n "${diff}" ]`
}
```
