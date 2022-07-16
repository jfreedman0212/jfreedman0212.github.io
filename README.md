# freedman.dev

This is the Hugo code for my personal blog.

# Running Locally

Putting this here so I don't forget again!

1. [Install Hugo](https://gohugo.io/getting-started/installing/)
2. Clone using the `--recurse-submodules` flag
    a. Or, if you don't clone with that flag, run this script after cloning:
    ```shell
    cd themes/hugo-ink
    git submodule update --init --recursive
    ```
3. Run `hugo server` to run a webserver locally and have it watch for changes

