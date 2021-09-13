# pilot

Pilot is an attractive, minimal, general purpose ggplot2 theme with an accessible discrete color palette.


## Installation

Install from GitHub using remotes.

``` r
install.packages("remotes")
remotes::install_github("olihawkins/pilot")
```

## Basic usage

Import the package and add `theme_pilot` to a plot made with `ggplot` in the normal way.

```r
library(ggplot2)
library(pilot)

plot <- ggplot(
        data = mpg,
        mapping = aes(
            x = displ, 
            y = hwy, 
            color = class)) + 
    geom_point() +
    labs(
        title = "Compact cars with smaller engines are more efficient",
        subtitle = "Engine size by fuel efficiency and class",
        x = "Engine size in litres",
        y = "Miles per gallon",
        color = "Class",
        caption = "Reproduced from Chapter 3 of R for Data Science") + 
    theme_pilot() +
    scale_color_pilot()
```

By default, ggplot2 horizontally aligns the title and subtitle with the left-hand edge of the plotting area. A helper function called `add_pilot_titles` is available that will align the title and subtitle with the left-hand edge of the x-axis. To use it, first create the plot in the normal way but without specifying the title and subtitle, then apply `add_pilot_titles` to add the titles to the plot. 

```r
library(ggplot2)
library(pilot)

plot <- ggplot(
        data = mpg,
        mapping = aes(
            x = displ, 
            y = hwy, 
            color = class)) + 
    geom_point() +
    labs(
        x = "Engine size in litres",
        y = "Miles per gallon",
        color = "Class",
        caption = "Reproduced from Chapter 3 of R for Data Science") + 
    theme_pilot() +
    scale_color_pilot()

plot <- add_pilot_titles(
    plot,
    title = "Compact cars with smaller engines are more efficient",
    subtitle = "Engine size by fuel efficiency and class")
```

Note that `add_pilot_titles` uses `patchwork` behind the scenes to compose a new plot from the existing plot and the titles. So if you want to adjust the plot margins using a `theme` customisation, do it *after* you have applied the titles, otherwise your customisation will be overwritten by `add_pilot_titles`.

## Colors

This theme includes an accessible discrete color palette, comprising seven colors that are visually distinct to people with the most commmon types of color blindness. These colors are available in a named vector called `pilot_colors`.

The base color names are:

* __navy__
* __blue__
* __brown__
* __green__
* __yellow__
* __purple__
* __orange__

You can use the `pilot_color` function to return the unnamed hex code value for a given name. This makes it easy to map specific colors to categorical variables using the `scale_color_manual()` and `scale_fill_manual()` functions.

```r
scale_color_manual(values = c(
    "a" = pilot_color("navy"),
    "b" = pilot_color("blue")))
```

These colors are also avaialable as ggplot2 scales with a range of palettes representing different subsets of the colors using `scale_color_pilot` and `scale_fill_pilot`. However, care should be taken in how you use these scales. For convenience these scales support ggplot2's color interpolation feature. But expanding the seven color palette to represent more than seven categories risks creating new colors that are no longer visually distinct to people with color blindness. The sequence of colours in the main palette has been chosen to reduce this risk, but if you want to ensure the colors remain distinct, you should only use these scales with discrete data that has the same number of categories as the palette you choose. More information on these scales can be found in the function reference below.

## Fonts

The theme uses a different default font on each desktop opertating system. These are "Avenir Next" on MacOS, "Trebuchet MS" on Windows, and the default sans-serif font on Linux. 

You can customise the fonts the theme uses in three ways: 

- Universally -- By setting family options for the theme in .Rprofile
- Per R session -- By setting the options dynamically using `options` or `set_pilot_family`
- Per plot -- By using the family arguments in `theme_pilot`

The full list of font family options that can be set in .Rprofile or using `options` are:

- `pilot.title_family`
- `pilot.subtitle_family`
- `pilot.axis_title_family`
- `pilot.axis_text_family`
- `pilot.legend_title_family`
- `pilot.legend_text_family`
- `pilot.facet_title_family`
- `pilot.caption_family`
- `pilot.geom_text_family`
- `pilot.annotate_family`

The `set_pilot_family` function sets all the font family options to the same family with a single function call.

```r
set_pilot_family("Helvetica Neue")
```

You can optionally set a different family for the main title.

```r
set_pilot_family("Avenir Next", title_family = "Avenir Next Demi Bold")
```

Use `?theme_pilot` to see the documentation showing the full list of arguments that can be used to customise the fonts for an individual plot using the arguments in `theme_pilot`.

## Function reference

### Themes

Apply the theme to a plot with `theme_pilot()`. There are a large number of arguments you can use to configure the components of the theme. Use `?theme_pilot` to see the full list of arguments. The principal arguments are documented below.

---

_pilot_::__theme_pilot__(_axes = ""_, _grid = "hv"_, _legend_position = "right"_, _caption_position = "right"_, _..._)

Set the theme with the following arguments:

* __axes__ A string indicating which axes should have lines and ticks. Specify which axes to show by including the matching characters in the string: "t" for top, "r" for right, "b" for bottom, "l" for left. You will need to ensure this argument is consistent with the axes settings in your plot for the lines and ticks to be displayed. The default is an empty string, meaning ticks and lines for the bottom and left axes are shown by default.
* __grid__ A string indicating which gridlines should be shown. Specify the gridlines to show by including the matching characters in the string: "h" for horizontal, "v" for vertical. The default is "hv", meaning both gridlines are shown by default.
* __legend_position__ A string indicating the position of the legend. Valid positions are "top", "right", "bottom", "left", "top-right", "top-left", "bottom-right", "bottom-left", and "none". The default is "right".
* __caption_position__ A string indicating the horizontal position of the caption. Valid positions are "left" or "right". The default is "right".

---

### Scales

Use `scale_color_pilot()` or `scale_fill_pilot()` as approriate. Both functions have the same signature. Please see the note on colors above for appropiate use of these scales.

---

_pilot_::__scale_color_pilot__(_palette = "main"_, _discrete = TRUE_, _reverse = FALSE_, _..._)
_pilot_::__scale_fill_pilot__(_palette = "main"_, _discrete = TRUE_, _reverse = FALSE_, _..._)

Sets the scales with the following arguments. The default palette is "five".

* __palette__ The name of a palette. Valid names are:
    * _two_ - navy, blue
    * _three_ - navy, blue, brown
    * _four_ - navy, blue, brown, green
    * _five_ - navy, blue, brown, green, yellow
    * _six_ - navy, blue, brown, green, yellow, purple
    * _seven_ - navy, blue, brown, green, yellow, purple, orange
* __discrete__ Boolean to indicate if color aesthetic is discrete.
* __reverse__ Boolean to indicate whether palette should be reversed.
* __...__ Additional arguments passed to `discrete_scale` or `scale_color_gradient`, depending on the value of `discrete`.

---

