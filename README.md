[![Build Status](https://travis-ci.org/blue-hood/git-sitemap.svg?branch=master)](https://travis-ci.org/blue-hood/git-sitemap)
[![Release](https://img.shields.io/github/release/blue-hood/git-sitemap.svg)](https://github.com/blue-hood/git-sitemap/releases/latest)
[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)](LICENSE)

# git-sitemap

Generate sitemap.xml with &lt;changefreq&gt; automatically🐥 based on the git history.

## Usage

Just run `git sitemap` on the git repository.
git-sitemap collects files under current directory.

(ex.)

```
$ git clone https://github.com/Hato6502/example-static-site
Cloning into 'example-static-site'...
remote: Enumerating objects: 259, done.
remote: Counting objects: 100% (259/259), done.
remote: Compressing objects: 100% (61/61), done.
remote: Total 259 (delta 111), reused 258 (delta 110), pack-reused 0
Receiving objects: 100% (259/259), 92.37 KiB | 286.00 KiB/s, done.
Resolving deltas: 100% (111/111), done.
$ cd example-static-site/
$ git sitemap | tee sitemap.xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://b-hood.site/articles/analog-orgel/</loc>
    <lastmod>2019-06-21T08:32:08+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>https://b-hood.site/articles/gitwitter/</loc>
    <lastmod>2019-06-21T08:32:08+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>https://b-hood.site/articles/husky/</loc>
    <lastmod>2019-06-21T08:32:08+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
  <url>
    <loc>https://b-hood.site/articles/</loc>
    <lastmod>2019-06-29T00:00:00+00:00</lastmod>
    <changefreq>always</changefreq>
    <priority>0.5</priority>
  </url>
  ...
  <url>
    <loc>https://b-hood.site/</loc>
    <lastmod>2019-06-21T08:32:08+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://b-hood.site/privacy/</loc>
    <lastmod>2019-06-21T08:32:08+00:00</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.5</priority>
  </url>
</urlset>
```

## Supports

- Linux
- mac OS

note: git-sitemap uses `date` command of GNU version, so please install GNU coreutils in your mac OS.

```
$ brew install coreutils
```

## Install

Just place /bin/git-sitemap on executable directory (e.g. /usr/local/bin).

## Settings

To customize git-sitemap, please place .git-sitemaprc.sh in the repository.
git-sitemap collects .git-sitemaprc.sh from the root directory of repository to each file's directory, and override settings.

### Domain

The domain is set to https://example.com by default.
This can be changed with PREFIX option.

(ex. )

```
PREFIX="https://b-hood.site"
```

### Target files

git-sitemap collects all files indexed by git by default, don't collect files listed in .gitignore.
By setting loc option, it can collect only html files.

```
function loc() {
  cat - | grep -E "\.html$"
}
```

And, it also can remove index.html with the following setting.

```
function loc() {
  cat - | grep -E "\.html$" | sed "s/index\.html$//"
}
```

### Update judgement

&lt;changefreq&gt; is judged based on the commit history of each file.
To count updated times correctly, please set difftest option.

(ex. 1) Judge changing files more than 10 lines as update.

```
function difftest () {
  # $1 the hash of commit
  # $2 file path

  local diff=`git diff -U0 $1^..$1 $2 | tail -n +5 | grep -E "^(\+|-)"`
  local count=`echo "${diff}" | wc -l`
  return `[ ${count} -ge 10 ]`
}
```

(ex. 2) Ignore the changes of css file's hash.

```
function difftest () {
  # $1 the hash of commit
  # $2 file path

  local diff=`git diff -U0 $1^..$1 -- $2 | tail -n +5 | grep -E "^(\+|-)"`
  # /css/app.css?id=99a477ea...
  diff=`echo "${diff}" | grep -v "\/css\/app\.css"`
  return `[ -n "${diff}" ]`
}
```

### Target commits

By setting SINCE option, it can restrict commits used to judge &lt;changefreq&gt;.
This is equals to `git log --since` option.

(ex. )

```
SINCE="1 month ago"
```

### Reference time

To generate sitemap at the specified time, please set CURRENT_TIME option.

(ex. )

```
CURRENT_TIME="2019-01-01"
```

### &lt;priority&gt; setting

It doesn't output &lt;priority&gt; by default.
This can be set as priority option.

(ex. )

```
function priority() {
  # $1 file path
  echo "0.5"
}
```

### &lt;lastmod&gt; &lt;changefreq&gt; filter

&lt;lastmod&gt; and &lt;changefreq&gt; are written automatically by default,
they can also be set specified value by lastmod and changefreq option.
Not to output these values, please define empty function.

(ex. ) Set &lt;lastmod&gt; to now, and &lt;changefreq&gt; to always forcibly.

```
function lastmod() {
  # $1 file path
  date --iso-8601 | w3cdatetime
}
function changefreq() {
  # $1 file path
  echo "always"
}
```

## Advanced

### Run on Travis CI

It is convenient to generate sitemap on Travis CI.
See [git-sitemap-travisci](https://github.com/Hato6502/git-sitemap-travisci) for details.

### Run with husky

It is also convenient to generate sitemap with husky.
See [git-sitemap-husky](https://github.com/Hato6502/git-sitemap-husky) for details.
