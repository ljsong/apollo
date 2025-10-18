# apollo

Modern and minimalistic blog theme powered by [Zola](https://getzola.org). See a live preview [here](https://not-matthias.github.io/apollo).

<sub><sup>Named after the greek god of knowledge, wisdom and intellect</sup></sub>

<details open>
  <summary>Dark theme</summary>

![blog-dark](./screenshot-dark.png)

</details>

<details>
  <summary>Light theme</summary>

![blog-light](./screenshot.png)

</details>

## Features

- [x] Pagination
- [x] Themes (light, dark, auto)
- [x] Projects page
- [x] Analytics using [GoatCounter](https://www.goatcounter.com/) / [Umami](https://umami.is/) / [Google Analytics](https://analytics.google.com/)
- [x] Social Links
- [x] MathJax Rendering
- [x] Taxonomies
- [x] Meta Tags For Individual Pages
- [x] Custom homepage
- [x] Comments
- [x] Search
- [x] RSS feeds
- [x] Mermaid diagram support
- [x] Table of Contents
- [x] Configurable cards layout

## Installation

1. Download the theme

```
git submodule add https://github.com/ljsong/apollo
```

2. Copy the  `config.toml` to zola root directory

3. Copy the example content

```
cp -r themes/apollo/content/* content/
```

## Configuration

Checkout all the [options you can configure](./content/posts/configuration.md) and the [example pages](./content/posts/).

## References

This theme is based on [archie-zola](https://github.com/XXXMrG/archie-zola/).
