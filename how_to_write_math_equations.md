## Github-PagesでのLaTeX形式の数式の記述方法

kramdownのmathjaxサポートを利用する

jekyll themeを使っている前提なので，Github-pagesでは使えるがGithubのデフォルトMarkdownでは綺麗にレンダリングできないはず．

[公式ドキュメント](https://help.github.com/articles/customizing-css-and-html-in-your-jekyll-theme/)のカスタマイズ方法を参考にする．


### krandownへの変更
```_config.yml```でmarkdown engineをkramdownに変更
```
theme: jekyll-theme-cayman
markdown: kramdown
```
### htmlの作成
```_layouts/default.html```
を作成し，https://github.com/pages-themes/THEME_NAME/blob/master/_layouts/default.html を開く．
```THEME_NAME```
は適時変更．上の場合は
```cayman```

### scriptの埋め込み
headの最後の辺りに
```
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
```
を追加．

### 文中で記述
```
$$
  x^2 - 6x + 1 = 0
$$
```




