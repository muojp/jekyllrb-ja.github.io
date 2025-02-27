---
title: Dataファイル
permalink: /docs/datafiles/
---
<!-- ---
title: Data Files
permalink: /docs/datafiles/
--- -->

Jekyllの[組み込み変数](../variables/)に加えて、[Liquid
templating system](https://wiki.github.com/shopify/liquid/liquid-for-designers){:target="_blank"}からアクセスできるカスタムデータを設定できます。

<!-- In addition to the [built-in variables](../variables/) available from Jekyll,
you can specify your own custom data that can be accessed via the [Liquid
templating system](https://wiki.github.com/shopify/liquid/liquid-for-designers). -->

Jekyllは`_data`ディレクトリの[YAML](http://yaml.org/){:target="_blank"}・[JSON](http://www.json.org/){:target="_blank"}・[CSV](https://en.wikipedia.org/wiki/Comma-separated_values){:target="_blank"}・[TSV](https://en.wikipedia.org/wiki/Tab-separated_values){:target="_blank"}ファイルのデータ読み込みをサポートしています。  
注：CSVとTSVはヘッダ行が*必要*です。

<!-- Jekyll supports loading data from [YAML](http://yaml.org/), [JSON](http://www.json.org/), [CSV](https://en.wikipedia.org/wiki/Comma-separated_values), and [TSV](https://en.wikipedia.org/wiki/Tab-separated_values) files located in the `_data` directory.
Note that CSV and TSV files *must* contain a header row. -->

この強力な機能は、`_config.yml`を変更することなく、テンプレートでの繰り返し処理やサイトの特別なオプションを提供します。

<!-- This powerful feature allows you to avoid repetition in your templates and to
set site specific options without changing `_config.yml`. -->

プラグインやテーマはデータファイルを利用して、設定用変数をセットできます。

<!-- Plugins/themes can also leverage Data Files to set configuration variables. -->

## データフォルダ
<!-- ## The Data Folder -->

`_data`フォルダがJekyllがサイトを生成するときに使う追加データを保管するフォルダです。ファイルは、YAMLかJSON、CSV（拡張子が`.yml`, `.yaml`, `.json`, `.csv`）ファイルで、`site.data`からアクセス可能になります。

<!-- The `_data` folder is where you can store additional data for Jekyll to use when
generating your site. These files must be YAML, JSON, or CSV files (using either
the `.yml`, `.yaml`, `.json` or `.csv` extension), and they will be
accessible via `site.data`. -->

## 例：メンバーのリスト
<!-- ## Example: List of members -->

大量のコードをJekyllテンプレートにコピー&ペースとしなくてすむように、データファイルの基本的な使用例を示します。

<!-- Here is a basic example of using Data Files to avoid copy-pasting large chunks
of code in your Jekyll templates: -->

In `_data/members.yml`:

```yaml
- name: Eric Mill
  github: konklone

- name: Parker Moore
  github: parkr

- name: Liu Fengyun
  github: liufengyun
```

Or `_data/members.csv`:

```text
name,github
Eric Mill,konklone
Parker Moore,parkr
Liu Fengyun,liufengyun
```

このデータは`site.data.members`でアクセスできます（注：ファイル名が変数名になります）。

<!-- This data can be accessed via `site.data.members` (notice that the filename
determines the variable name). -->

これで、次のようテンプレートに記述すると、メンバーのリスト作成できます。

<!-- You can now render the list of members in a template: -->

{% raw %}
```liquid
<ul>
{% for member in site.data.members %}
  <li>
    <a href="https://github.com/{{ member.github }}">
      {{ member.name }}
    </a>
  </li>
{% endfor %}
</ul>
```
{% endraw %}

## サブフォルダ
<!-- ## Subfolders -->

データファイルは`_data`フォルダのサブフォルダに配置することもできます。各フォルダのレベルが変数のネームスペースとして追加されます。GitHub組織が`orgs`フォルダの各ファイルで設定されている場合の例を、以下にしまします。

<!-- Data files can also be placed in sub-folders of the `_data` folder. Each folder
level will be added to a variable's namespace. The example below shows how
GitHub organizations could be defined separately in a file under the `orgs`
folder: -->

In `_data/orgs/jekyll.yml`:

```yaml
username: jekyll
name: Jekyll
members:
  - name: Tom Preston-Werner
    github: mojombo

  - name: Parker Moore
    github: parkr
```

In `_data/orgs/doeorg.yml`:

```yaml
username: doeorg
name: Doe Org
members:
  - name: John Doe
    github: jdoe
```

組織には、`site.data.orgs`にファイル名を続けてアクセスすることができます。

<!-- The organizations can then be accessed via `site.data.orgs`, followed by the
file name: -->

{% raw %}
```liquid
<ul>
{% for org_hash in site.data.orgs %}
{% assign org = org_hash[1] %}
  <li>
    <a href="https://github.com/{{ org.username }}">
      {{ org.name }}
    </a>
    ({{ org.members | size }} members)
  </li>
{% endfor %}
</ul>
```
{% endraw %}

## 例：特定の著者へのアクセス
<!-- ## Example: Accessing a specific author -->

ページやポストもデータアイテムにアクセスすることができます。以下にその例を示します。

<!-- Pages and posts can also access a specific data item. The example below shows how to access a specific item: -->

`_data/people.yml`:

```yaml
dave:
    name: David Smith
    twitter: DavidSilvaSmith
```

ポストのfront matterでページ変数として著者（author）を指定できます。

<!-- The author can then be specified as a page variable in a post's front matter: -->

{% raw %}
```liquid
---
title: sample post
author: dave
---

{% assign author = site.data.people[page.author] %}
<a rel="author"
  href="https://twitter.com/{{ author.twitter }}"
  title="{{ author.name }}">
    {{ author.name }}
</a>
```
{% endraw %}

（特にドキュメンテーションサイトや多くのページが存在するJekyllサイトを持っているなら）しっかりしたナビゲーションの構築のための情報は、[ナビゲーション]({{ "tutorials/navigation" | relative_url }})を見てください。

<!-- For information on how to build robust navigation for your site (especially if you have a documentation website or another type of Jekyll site with a lot of pages to organize), see [Navigation](/tutorials/navigation). -->
