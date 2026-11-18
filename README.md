# azade.dev

[azade.dev](https://azade.dev) is Azade's site: who she is, and who François is. It is a Rails app that never serves a request in production. [Parklife](https://github.com/benpickles/parklife) crawls it and writes static files, which GitHub Pages serves. The approach comes from [Rails Static](https://fcatuhe.github.io/rails-static), and the bilingual setup from the first skyblip.eu.

## Running it

```sh
mise trust && mise install   # Ruby
bin/setup                    # gems, then bin/dev
bin/dev                      # http://azade.localhost:4230
bin/ci                       # rubocop, bundler-audit, importmap audit, brakeman
```

## Content

Pages are files, not database rows: `content/pages/<slug>.html.erb`, read by [Decant](https://github.com/benpickles/decant), with YAML frontmatter for `title`, `nav_title`, `description` and an optional localized `slug`. A page is bilingual by convention, `<slug>.html.erb` in English and `<slug>.fr.html.erb` in French. English lives at `/`, French under `/fr`. The locale switcher offers a language only where the twin exists, each page links its twins with `hreflang`, and a first visit lands on the visitor's browser language, which a click on the switcher overrides for a year.

Parklife discovers pages by crawling from the root, so a page nothing links to is not built. Link it from the nav (`app/views/layouts/_nav.html.erb`) or from another page's body.

The words the layout itself speaks live in `config/locales/`. Images live in `content_images/pages/<slug>/` and are drawn with `pages_image_tag`. Colors, spacing and type are CSS variables in `app/assets/stylesheets/_global.css`.

## Building and deploying

```sh
bin/static-build --base https://azade.dev   # -> build/
```

`bin/static-build` precompiles assets, runs Parklife, which also writes the sitemap and `robots.txt`, then copies `public/` over the result. Run `rm -rf public/assets` before going back to `bin/dev`, or the development server serves the precompiled assets.

`.github/workflows/pages.yml` runs `bin/ci` and the build on every pull request, and deploys to GitHub Pages from `main`. The custom domain is set in the repository's Pages settings.

## License

MIT, see [`LICENSE`](LICENSE). The photo is a Pyrenean goat of Ferme Bacotte, [Agriculture Poétique](https://www.instagram.com/agriculturepoetique), Saint-Sever, and is not under the MIT license.
