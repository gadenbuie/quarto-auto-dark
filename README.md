# Auto Dark Mode for Quarto


Bring automatic dark mode to Quarto web documents (webpages, slides,
articles, etc.).

## Installing

``` bash
quarto add gadenbuie/quarto-auto-dark
```

This will install the extension under the `_extensions` subdirectory. If
you’re using version control, you will want to check in this directory.

## Using

To use the extension, include `auto-dark` in the list of filters in your
document metadata or in your `_quarto.yml` configuration file.

``` yaml
filters:
  - auto-dark
```

When a user visits your webpage or slides at night — or whenever their
operating system is set to use dark mode — an automatic dark mode is
applied.

For best results in Quarto webpages, choose a single light-based theme
for your page or site, and do not use [Quarto’s dark mode
feature](https://quarto.org/docs/websites/website-tools.html#dark-mode).

``` yaml
# Do this
theme:
  light: flatly

# Don't do this
theme:
  light: flatly
  dark: darkly
```

## Example

This page is an example! Try changing your system settings to dark mode,
or just come back to this page tomorrow or later tonight, to see auto
dark mode in action.

## About

Quarto auto dark mode builds on a CSS-only approach to dark mode
outlined by [Aral Balkan](https://ar.al/) in his post [Implementing dark
mode in a handful of lines of CSS with CSS
filters](https://ar.al/2021/08/24/implementing-dark-mode-in-a-handful-of-lines-of-css-with-css-filters/).

Auto dark mode does not provide a light/dark switch and we recommend you
do not include one. Instead follow the user’s operating system. All
phones, tablets and modern operating systems provide conveniences to
choose between light and dark user interfaces.

Our view is that a light/dark switch is only necessary if your users
need to choose a color scheme that differs from the state of their
operating system, which is increasingly unlikely for most websites.

At its core, auto dark mode is built on two key ideas:

1.  First, it uses a `prefers-color-scheme: dark` CSS media query to set
    styles when the operating system is in dark mode. You can use this
    approach in your own CSS styles:

    ``` css
    /* Make bold things red by default */
    strong {
      color: "red"
    }

    @media (prefers-color-scheme: dark) {
     /* In dark mode, use blue for bold text */
     strong {
       color: "blue"
     }
    }

    @media (prefers-color-scheme: light) {
      /* You can also be specific about light mode */
      strong {
       color: "green"
      }
    }
    ```

2.  In dark mode, auto dark mode inverts all of the colors on the page,
    while trying to keep hue consistent.

    ``` css
    @media (prefers-color-scheme: dark) {
      /* Invert all elements on the body while attempting to not alter the hue substantially. */
      body {
        filter: invert(100%) hue-rotate(180deg);
      }
    }
    ```

    This is definitely a big hammer approach to dark mode, but in most
    cases it works well and you can read more about it in [Aral’s blog
    post](https://ar.al/2021/08/24/implementing-dark-mode-in-a-handful-of-lines-of-css-with-css-filters/).

    If you want a particular element to retain its original colors,
    apply the same filter rule to repeat the inversion. Auto dark mode
    does this for images, SVGs and icons by default.

    ``` css
    @media (prefers-color-scheme: dark) {
      /* Do not invert media (revert the invert). */
      img, video, iframe, svg:not(.bi, .fa) {
        filter: invert(100%) hue-rotate(180deg);
      }
    }
    ```
