+++
template = "homepage.html"
+++

<style>
.homepage-hero {
    text-align: center;
    padding: 2rem 0;
}

.homepage-hero-title {
    font-size: 3rem;
    margin-bottom: 1rem;
}

.homepage-hero-subtitle {
    font-size: 1.25rem;
    margin-bottom: 1rem;

</style>

<div class="homepage-hero">
    <h1 class="homepage-hero-title">Alex Song</h1>
    <p class="homepage-hero-subtitle">欢迎来到我的Blog，这里主要记录一些我的日常学习内容，包括机器学习、程序设计、数学以及各种技术和非技术的相关内容。 <br/><br/>如非特别指明，本站的文章遵循 <a href = "https://creativecommons.org/licenses/by-nc-sa/4.0/">Attribution-NonCommercial-ShareAlike Creative Commons</a> 协议。 更多文章索引请浏览 <a href="https://alex-song.com/posts">Posts</a>, <a href="https://alex-song.com/categories">Categories</a>, 和 <a href="https://alex-song.com/tags">Tags</a>。 <a href="https://alex-song.com/collections">Collections</a>中会记录一些有意思的书籍、游戏和电影。</p>
</div>

# Features

- [Light, dark, and auto themes](@/posts/configuration.md#theme-mode-theme)
- [Projects page](@/projects/_index.md)
- [Talks page](https://not-matthias.github.io/talks/)
- [Analytics (GoatCounter, Umami)](@/posts/configuration.md#analytics)
- [Social media links](@/posts/configuration.md#socials)
- [MathJax rendering](@/posts/math-symbol.md)
- [Taxonomies](/apollo/tags)
- [Custom homepage](@/posts/custom-homepage.md)
- [Comments](@/posts/configuration.md#comments-comment)
- [Search functionality](@/posts/configuration.md#search-build-search-index)

Checkout all the [options you can configure](@/posts/configuration.md) and the [example pages](@/posts/_index.md).

# Quick Start

1.  **Add the theme as a submodule:**
    ```bash
    git submodule add https://github.com/not-matthias/apollo themes/apollo
    ```
2.  **Configure your `config.toml`:**
    Set `theme = "apollo"` and add your site's configuration.
3.  **Start the Zola server:**
    ```bash
    zola serve
    ```
