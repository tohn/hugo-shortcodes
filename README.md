# Hugo Shortcodes

Some [shortcodes][hugo_shortcodes] for [Hugo][], which you can add to
your website as a [Hugo Module][hugo_mods] :sparkles:

## Install

1. Enable modules in your repository (**skip this step** if you already
   have a `go.mod` file in your repo):

   ```bash
   hugo mod init github.com/$username/$reponame
   ```

2. Add this module to your website by adding it to your `config.toml`:

   ```toml
   [module]
     [[module.imports]]
       path = "github.com/tohn/hugo-shortcodes"
   ```

3. Run Hugo. This will download the latest version of the module.

   ```bash
   hugo server -v -D
   ```

## Update

You can update this module with:

```bash
hugo mod get -u github.com/tohn/hugo-shortcodes
```

Or you can update **all** modules defined in your `config.toml` with:

```bash
hugo mod get -u
```

## Styling

I decided to **not** include the (S)CSS directly in the shortcode so you
have to include it yourself. This is due to [minification][hugo_minify]
and [bundling reasons][hugo_bundle].

Include it like this in your `<head>` [in
`layouts/_default/baseof.html`][hugo_baseof]:

```html
<head>
  <!-- CSS -->
  {{ $fontawesome := resources.Get "css/fontawesome.scss" | toCSS }}
  {{ $spoiler := resources.Get "css/spoiler.scss" | toCSS }}
  {{ $css := slice $fontawesome $spoiler | resources.Concat "css/bundle.css" | minify }}
  {{ if ne hugo.Environment "development" -}}
    {{ $css = $css | fingerprint -}}
  {{ end -}}
  <link rel="stylesheet" href="{{ $css.Permalink }}">
```

Adjust the variables you need/want in the line with `$css := slice`.

## Override

You can override a shortcode by adding a file with the same name as the
one you want to replace in your own shortcode directory
(`layouts/shortcodes` for example).

## Modules

### icon.html

This shortcode can be used to include a fontawesome icon to your
website.

To use it, we first have to add the Hugo Module for
fontawesome. Normally we can just declare the module in `config.toml`.
But since this would add [an outdated release][go_reddit] (`4.x` instead
of `6.x`), we have to add this via this command (notice the `@6.x` at
the end):

```bash
hugo mod get -u github.com/fontawesome/font-awesome@6.x
```

Afterwards we have to add the module in our `config.toml`:

```toml
[module]
  [[module.imports]]
    path = "github.com/FortAwesome/Font-Awesome"
    [[module.imports.mounts]]
      source = "scss"
      target = "assets/css/fontawesome"
    [[module.imports.mounts]]
      source = "webfonts"
      target = "static/fonts/fontawesome
```

We also have to add the SCSS in
[assets/css/fontawesome.scss][fontawesome_scss]. See [Styling][] how to
do this.

Example:

```go
{{< icon size="fa-lg" anim="fa-spin" col="#abcdef" >}}
{{< icon set="fa-brands" icon="fa-github" rot="flip-both" anim="fa-fade" >}}
```

### spoiler.html

This shortcode is essentially copied [from Nelis Oostens][spoiler_src]
(thanks for this!).

It can be used to hide some text (inline or multiline) and reveal it by
hovering over it.

Example:

```go
CN Spoiler {{< spoiler >}}Secret!!1!{{< /spoiler >}}

{{< spoiler >}}
Fictum, deserunt mollit anim laborum astutumque! Quisque placerat
facilisis egestas cillum dolore. Nec dubitamus multa iter quae et nos
invenerat. Contra legem facit qui id facit quod lex prohibet. Quam diu
etiam furor iste tuus nos eludet?
{{< /spoiler >}}
```

You can find the SCSS in [assets/css/spoiler.scss][spoiler_scss]. See
the instructions in [Styling][] how to add this to your website.

## Inspirations

* <https://github.com/aormsby/hugo-shortcodes>
* <https://github.com/dnb-org/dnb-hugo-shortcodes>
* <https://github.com/parsiya/Hugo-Shortcodes>

[Hugo]: https://gohugo.io
[hugo_mods]: https://gohugo.io/hugo-modules/
[hugo_shortcodes]: https://gohugo.io/content-management/shortcodes/
[hugo_minify]: https://gohugo.io/hugo-pipes/minification/
[hugo_bundle]: https://gohugo.io/hugo-pipes/bundling/
[hugo_baseof]: https://gohugo.io/templates/base/#define-the-base-template
[spoiler_src]: https://oostens.me/posts/hugo-inline-spoiler-shortcode/
[spoiler_scss]: ./assets/css/spoiler.scss
[Styling]: #styling
[go_reddit]: https://www.reddit.com/r/golang/comments/b1d0rp/go_mod_issues_getting_the_exact_latest_version/
[fontawesome_scss]: ./assets/css/fontawesome.scss
