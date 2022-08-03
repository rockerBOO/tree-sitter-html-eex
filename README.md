# tree-sitter-html-eex

HTML grammar with support for `eex` template injections for [tree-sitter][].

## Background

This parser is a fork for tree-sitter-html with support for templating languages, like `eex` inside Elixir. This allows us to do proper language injections inside Neovim for `*.html.eex` type files.

![screenshot of neovim showing html and eex together](https://user-images.githubusercontent.com/15027/182512287-a50d918e-b8f5-4a4d-84d2-afbfaa5befc8.png)

## Usage

### Neovim

You will need the `eex` parser that's available through `nvim-treesitter`

```lua
local parser_config =
	require("nvim-treesitter.parsers").get_parser_configs()

parser_config.html_eex = {
	install_info = {
    url = "https://github.com/rockerBOO/tree-sitter-html-eex",
		files = { "src/parser.c", "src/scanner.cc" },
	},
	maintainers = {  "@rockerBOO" },
}
```

Then we need some queries for nvim-treesitter.

`~/.config/nvim/queries/html_eex/highlights.scm`

```scheme
;inherits: html
```

`~/.config/nvim/queries/html_eex/injections.scm`

```scheme
;inherits: html
((template) @eex)
```

Now we can install the `html_eex` parser via nvim-treesitter.

```vim
:TSInstallFromGrammar html_eex
```

And in `~/.config/nvim/ftdetect/html_eex.lua` we need to add an autocmd to activate for `*.html.eex` files.

```lua
vim.api.nvim_create_autocmd({"BufRead", "BufNewFile"}, {
  pattern = "*.html.eex",
  command = "set filetype=html_eex"
})
```

If done properly, when opening a file like `examples/text.html.eex` should open as a `html_eex` format and show both HTML and EEx highlighting.

## Development

`npm install`

`npm run build`

`npm run test`

### Reload tests on save

`fd | grep -E '(txt$|js$)' | entr sh -c 'tree-sitter generate && tree-sitter test'`

Useful when updating the test or grammar. Will regenerate and test again. Requires [fd](https://github.com/sharkdp/fd) and [entr](https://github.com/eradman/entr) but you could use whatever you like to reload on save.

[tree-sitter]: https://github.com/tree-sitter/tree-sitter

References

- [The HTML5 Spec](https://www.w3.org/TR/html5/syntax.html)
