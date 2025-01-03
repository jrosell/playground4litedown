playground4litedown
===================

This repo exists because of [this toot in
mastodon](https://mastodon.social/@defuneste@fosstodon.org/113398806612007032)
requesting help to build a small
[{litedown}](https://github.com/yihui/litedown) site.

Installation
------------

``` r
pak::pak(c("yihui/litedown", "yihui/servr"))
```

Example
-------

I list multiple options here:

-   Render the site from one place, and serve it from another place.

``` r
litedown::fuse_site("www1")
from_files <- c(
  fs::dir_ls("www1/src", glob = "*.html"),
  fs::dir_ls("www1/src", glob = "*_files"),
  fs::dir_ls("www1/src", glob = "*_cache")
)
fs::file_move(from_files, "www1")
servr::httw("www1", ".", handler = \(x) {
    cat(paste(x), " changed", '\n')
    litedown::fuse_site(".")
    from_files <- c(
      fs::dir_ls("src", glob = "*.html"),
      fs::dir_ls("src", glob = "*_files"),
      fs::dir_ls("src", glob = "*_cache"),
    )
    fs::file_move(from_files, ".")
})
```

-   Render the site and serve it from the same place, watching for
    changes to re-render.

``` r
litedown::fuse_site("www2")
servr::httw("www2", ".", handler = \(x) {
    cat(paste(x), " changed", '\n')
    litedown::fuse_site(".")
})
```

-   Launch a web page to list and preview R Markdown and HTML files
    under the same directory.

``` r
litedown::fuse_site("www3")
litedown::roam("www3") 
```
