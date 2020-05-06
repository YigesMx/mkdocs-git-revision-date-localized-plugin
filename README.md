[![Actions Status](https://github.com/timvink/mkdocs-git-revision-date-localized-plugin/workflows/pytest/badge.svg)](https://github.com/timvink/mkdocs-git-revision-date-localized-plugin/actions)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/mkdocs-git-revision-date-localized-plugin)
![PyPI](https://img.shields.io/pypi/v/mkdocs-git-revision-date-localized-plugin)
![PyPI - Downloads](https://img.shields.io/pypi/dm/mkdocs-git-revision-date-localized-plugin)
[![codecov](https://codecov.io/gh/timvink/mkdocs-git-revision-date-localized-plugin/branch/master/graph/badge.svg)](https://codecov.io/gh/timvink/mkdocs-git-revision-date-localized-plugin)
![GitHub contributors](https://img.shields.io/github/contributors/timvink/mkdocs-git-revision-date-localized-plugin)
![PyPI - License](https://img.shields.io/pypi/l/mkdocs-git-revision-date-localized-plugin)

# mkdocs-git-revision-date-localized-plugin

[MkDocs](https://www.mkdocs.org/) plugin that enables displaying the date of the last git modification of a page. The plugin uses [babel](https://github.com/python-babel/babel/tree/master/babel) and [timeago.js](https://github.com/hustcc/timeago.js) to provide different localized date formats. Initial fork from [mkdocs-git-revision-date-plugin](https://github.com/zhaoterryy/mkdocs-git-revision-date-plugin).

![demo](demo_screencast.gif)

(*Example when used together with the [mkdocs-material](https://github.com/squidfunk/mkdocs-material) theme*)

## Setup

Install the plugin using `pip3` with the following command:

```bash
pip3 install mkdocs-git-revision-date-localized-plugin
```

Next, add the following lines to your `mkdocs.yml`:

```yaml
plugins:
  - search
  - git-revision-date-localized
```

> If you have no `plugins` entry in your config file yet, you'll likely also want to add the `search` plugin. MkDocs enables it by default if there is no `plugins` entry set.

### Note when using on CI runners

The plugin needs access to the last commit that touched a file to be able to retrieve the date. If you build your docs using CI then you might need to change your settings:

- github actions: set `fetch_depth` to `0` ([docs](https://github.com/actions/checkout))
- gitlab runners: set `GIT_DEPTH` to `1000` ([docs](https://docs.gitlab.com/ee/user/project/pipelines/settings.html#git-shallow-clone))

----

## Usage

### In supported themes

- [mkdocs-material](https://squidfunk.github.io/mkdocs-material/) offers native support for this plugin, see [setup instructions](https://squidfunk.github.io/mkdocs-material/plugins/revision-date/)

### In markdown pages

In your markdown files you can use the `{{ git_revision_date_localized }}` tag anywhere you'd like:

```django hljs
Last update: {{ git_revision_date_localized }}
```

### In custom themes

When writing your own [custom themes](https://www.mkdocs.org/user-guide/custom-themes/) you can use the `page.meta.git_revision_date_localized` jinja tag:

```django hljs
{% if page.meta.git_revision_date_localized %}
  Last update: {{ page.meta.git_revision_date_localized }}
{% endif %}
```

## Options

You can customize the plugin by setting options in `mkdocs.yml`. For example:

```yml
plugins:
  - git-revision-date-localized:
    type: timeago
    locale: en
    fallback_to_build_date: false
```

### `type`

Default is `date`. To change the date format, set the `type` parameter to one of `date`, `datetime`, `iso_date`, `iso_datetime` or `timeago`. Example outputs:

```bash
28 November, 2019           # type: date
28 November, 2019 13:57:28  # type: datetime
2019-11-28                  # type: iso_date
2019-11-28 13:57:26         # type: iso_datetime
20 hours ago                # type: timeago
```

### `locale`

Default is `None`. Specify a two letter [ISO639](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) language code to display dates in your preferred language.

- When not set, this plugin will look for `locale` or `language` options set in your theme. If also not set, the fallback is English (`en`)
- When used in combination with `type: date` or `type: datetime`, translation is done using [babel](https://github.com/python-babel/babel) which supports [these locales](http://www.unicode.org/cldr/charts/latest/supplemental/territory_language_information.html)
- When used in combination with `type: timeago` then [timeago.js](https://github.com/hustcc/timeago.js) is added to your website, which supports [these locales](https://github.com/hustcc/timeago.js/tree/master/src/lang). If you specify a locale not supported by timeago.js, the fallback is English (`en`)

### `fallback_to_build_date`

Default is `false`. If set to `true` the plugin will use the time when running `mkdocs build` instead of the git revision date. This means the revision date will be inaccurate, but this can be useful if your build environment has no access to GIT and you want to ignore the Git exceptions during `git log`.

## Contributing

Contributions are very welcome! Please read [CONTRIBUTING.md](CONTRIBUTING.md) before putting in any work.