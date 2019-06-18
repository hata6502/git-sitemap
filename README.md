# git-sitemap

git 管理による静的サイトのサイトマップを作成します。

## 使い方

サイトマップを作成する git リポジトリにて `git sitemap` コマンドを実行します。

(例) ```
$ git add .
$ git commit -m "記事の執筆者名を修正しました。"
$ git sitemap > sitemap.xml
$ cat sitemap.xml
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;urlset xmlns=&quot;http://www.sitemaps.org/schemas/sitemap/0.9&quot;&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/analog-orgel/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;daily&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/gitwitter/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/mvhash/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/pre_commit/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/sitemap/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/staticgen/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/sticky-stack/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/table-of-contents/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/trimrate/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/articles/webp/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T22:06:30+09:00&lt;/lastmod&gt;&lt;changefreq&gt;weekly&lt;/changefreq&gt;&lt;/url&gt;
&lt;url&gt;&lt;loc&gt;https://b-hood.site/privacy/&lt;/loc&gt;&lt;lastmod&gt;2019-06-18T00:16:13+09:00&lt;/lastmod&gt;&lt;changefreq&gt;monthly&lt;/changefreq&gt;&lt;/url&gt;
&lt;/urlset&gt;

```

## インストール
`/usr/local/bin` などに `git-sitemap` を配置し、実行権限を付与します。

## 設定
サイトマップを作成する git リポジトリにて、以下のコマンドでサイトのドメインを設定します。

```

\$ git config sitemap.domain "https://example.com"

```

## サイトマップへの登録対象
git へのインデックスが作られている `.html` ファイルが対象となります。
なお、`.gitignore` によって無視されたページは登録されません。

## changefreq, lastmod の生成
changefreq, lastmod は、git のコミット履歴をもとに自動生成されます。
```
