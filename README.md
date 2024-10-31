# playground4litedown

This repo exists because of [this toot in
mastodon](https://mastodon.social/@defuneste@fosstodon.org/113398806612007032)
asking for help building a small
[{litedown}](https://github.com/yihui/litedown) site.

## Installation

``` r
pak::pak(c("yihui/litedown", "yihui/servr"))
```

## Example

Render the site and serve it whatching changes for rerendering.

``` r
litedown::fuse_site("www")
servr::httw("www", ".", handler = \(x) {
    cat(paste(x), " changed", '\n')
    litedown::fuse_site(".")
})
```
