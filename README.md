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
  {{ $tohn_hugo_shortcodes := resources.Get "css/tohn_hugo_shortcodes.scss" | toCSS }}
  {{ $css := slice $tohn_hugo_shortcodes | resources.Concat "css/bundle.css" | minify }}
  {{ if ne hugo.Environment "development" -}}
    {{ $css = $css | fingerprint -}}
  {{ end -}}
  <link rel="stylesheet" href="{{ $css.Permalink }}">
```

Adjust the styles you need/want by copying
[`assets/css/tohn_hugo_shortcodes.scss`][scss] to your `assets/css`
folder and deleting the lines you don't need/want.

## i18n

To also support websites with more than one language (or a different one
than german or english), we use the [i18n function][hugo_i18n] of Hugo.
Read more about the [Multilingual Mode][hugo_multilingual] in the docs.

If you want to contribute some translations, feel free to do so :blush:  
Have a look in the [i18n folder][i18n] and use the file `en.yaml` as a
starting point.

## Override

You can override a shortcode by adding a file with the same name as the
one you want to replace in your own shortcode directory
(`layouts/shortcodes` for example). The same works for SCSS or i18n
files.

## Modules

### abbr.html

[`abbr.html`][abbr] is copied from [sametbh][].

It can be used to insert an [`abbr` HTML tag][html_abbr] into your page.

Example:

```go
{{< abbr title="What The Fuck" text="WTF" >}}
```

### audio.html

[`audio.html`][audio] can be used for inserting audio files with the
[audio HTML tag][html_audio] into your page.

This shortcode makes use of [i18n](#i18n).

Example:

```go
{{< audio src="example.ogg" src2="example.mp3" >}}
```

### color.html

[`color.html`][color] can be used to display the color representation
of a [hex value][color_hex] in your text.

This is one of the (hopefully!) few shortcodes with CSS declarations in
it (since it would be very impractical to define every hex value
beforehand).

Example:

```go
{{< color "#1db954" >}}
```

### contrastchecker.html

[`contrastchecker.html`][contrastchecker] uses the [Contrast Checker by
WebAIM][webaim_cc] to display the Contrast Ratio and Passings/Failings
of [WCAG AA(A)][WCAG] of given Foreground and Background Colors in a
table.

Of course you can use [CSS to style the table][css_table].

Example:

```go
{{< contrastchecker fcolor="242424" bcolor="FCFCFC" >}}
```

### linkcontrastchecker.html

[`linkcontrastchecker.html`][linkcontrastchecker] is similar to
[contrastchecker.html](#contrastcheckerhtml) and uses the [Link Contrast
Checker by WebAIM][webaim_lcc] to display some ratios of given Link,
Body Text and Background Color in a table.

I used a [tables generator][tables_generator] to help me create the
table.

Example:

```go
{{< linkcontrastchecker fcolor="242424" bcolor="FCFCFC" lcolor="0A802D" >}}
```

### icon.html

[`icon.html`][icon] can be used to include [a fontawesome
icon][fontawesome] to your website.

To use it, we first have to add the Hugo Module for
fontawesome. Normally we can just declare the module in `config.toml`.
But since this would add [an outdated release][go_reddit] (`4.x` instead
of `6.x`), we have to add this via this command (notice the `@6.x` at
the end):

```bash
hugo mod get github.com/FortAwesome/Font-Awesome@6.x
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
      target = "static/fonts/fontawesome"
```

We also have to add the SCSS in
[`assets/css/fontawesome.scss`][fontawesome_scss]. See [Styling][] how
to do this.

Example:

```go
{{< icon size="fa-lg" anim="fa-spin" col="#abcdef" >}}
{{< icon set="fa-brands" icon="fa-github" rot="flip-both" anim="fa-fade" >}}
```

### progress.html

[`progress.html`][progress] can be used to display a progress bar on
your page. It is inspired by [W3.CSS Progress Bars][w3] and
[Bootstrap][].

Example:

```go
{{< progress cur=3 max=42 color="#bee" mode="percent" >}}
```

### spoiler.html

[`spoiler.html`][spoiler] is essentially copied [from Nelis
Oostens][spoiler_src] (thanks for this!).

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

You can find the SCSS in [`assets/css/spoiler.scss`][spoiler_scss]. See
the instructions in [Styling][] how to add this to your website.

### sub.html

Since we can't have HTML code in markdown, [`sub.html`][sub] can be used
to use the [`sub` HTML tag][html_sub].

Example:

```go
CO{{< sub "2" >}}
```

### sup.html

Similar to [sub.html](#subhtml), [`sup.html`][sup] can be used to use the
[`sup` HTML tag][html_sup].

Example:

```go
2{{< sup "10" >}}
```

### video.html

[`video.html`][video] can be used for inserting video files with the
[video HTML tag][html_video] into your page.

An alternative for this shortcode could be
[hugo-video][video_alternative].

This shortcode makes use of [i18n](#i18n).

Example:

```go
{{< video src="example.webm" >}}
```

### wayback.html

[`wayback.html`][wayback] uses the [Wayback Machine][wayback_machine] to
simplify the process of reviving links to deleted/moved or otherwise now
inaccessible sources. Thanks [Fryboyter for the
inspiration][wayback_inspiration]!

I'm using this shortcode with [reference style links][rsl] in markdown.
It's also important to notice that we have to use a [different shortcode
notation][shortcode_notation] (with `%` instead of `<` and `>`).

Example:

```md
This is [a link][1].

[1]: {{% wayback "https://example.org" %}}
```

## Inspirations

* <https://github.com/aormsby/hugo-shortcodes>
* <https://github.com/dnb-org/dnb-hugo-shortcodes>
* <https://github.com/parsiya/Hugo-Shortcodes>

[Bootstrap]: https://getbootstrap.com/docs/5.2/components/progress/
[Hugo]: https://gohugo.io
[Styling]: #styling
[WCAG]: https://www.w3.org/WAI/standards-guidelines/wcag/
[abbr]: ./layouts/shortcodes/abbr.html
[audio]: ./layouts/shortcodes/audio.html
[color]: ./layouts/shortcodes/color.html
[color_hex]: https://htmlcolorcodes.com
[contrastchecker]: ./layouts/shortcodes/contrastchecker.html
[css_table]: https://www.w3schools.com/css/css_table.asp
[fontawesome]: https://fontawesome.com
[fontawesome_scss]: ./assets/css/fontawesome.scss
[go_reddit]: https://www.reddit.com/r/golang/comments/b1d0rp/go_mod_issues_getting_the_exact_latest_version/
[html_abbr]: https://www.w3schools.com/tags/tag_abbr.asp
[html_audio]: https://www.w3schools.com/tags/tag_audio.asp
[html_sub]: https://www.w3schools.com/tags/tag_sub.asp
[html_sup]: https://www.w3schools.com/tags/tag_sup.asp
[html_video]: https://www.w3schools.com/tags/tag_video.asp
[hugo_baseof]: https://gohugo.io/templates/base/#define-the-base-template
[hugo_bundle]: https://gohugo.io/hugo-pipes/bundling/
[hugo_i18n]: https://gohugo.io/functions/i18n/
[hugo_minify]: https://gohugo.io/hugo-pipes/minification/
[hugo_mods]: https://gohugo.io/hugo-modules/
[hugo_multilingual]: https://gohugo.io/content-management/multilingual/
[hugo_shortcodes]: https://gohugo.io/content-management/shortcodes/
[i18n]: ./i18n
[icon]: ./layouts/shortcodes/icon.html
[linkcontrastchecker]: ./layouts/shortcodes/linkcontrastchecker.html
[progress]: ./layouts/shortcodes/progress.html
[rsl]: https://www.markdownguide.org/basic-syntax/#reference-style-links
[sametbh]: https://www.sametbh.com/docs/64-programming/ides/atom/atom-hugo-shortcodes-snippets/
[scss]: ./assets/css/tohn_hugo_shortcodes.scss
[shortcode_notation]: https://discourse.gohugo.io/t/using-shortcode-inside-render-hook/42038/5
[spoiler]: ./layouts/shortcodes/spoiler.html
[spoiler_scss]: ./assets/css/spoiler.scss
[spoiler_src]: https://oostens.me/posts/hugo-inline-spoiler-shortcode/
[sub]: ./layouts/shortcodes/sub.html
[sup]: ./layouts/shortcodes/sup.html
[tables_generator]: https://www.tablesgenerator.com/html_tables
[video]: ./layouts/shortcodes/video.html
[video_alternative]: https://github.com/martignoni/hugo-video
[w3]: https://www.w3schools.com/w3css/w3css_progressbar.asp
[wayback]: ./layouts/shortcodes/wayback.html
[wayback_inspiration]: https://fryboyter.de/hugo-tote-links-mit-der-wayback-machine-wiederbeleben/
[wayback_machine]: https://archive.org
[webaim_cc]: https://webaim.org/resources/contrastchecker/
[webaim_lcc]: https://webaim.org/resources/linkcontrastchecker/
