# Season Plot Package

Plot seasonal components of a time-series by frequency groups.

This package computes and depicts a time-series across sub-periods, also called a Buys-Ballot plot. For instance, if one has a quarterly time-series one can plot the dynamics of each quarter across years. Vice versa, you may also be interested in plotting the dynamics of each year's quarter.

Note, you may also try the user-written gretl package "buys_ballot" written by Ignacio Diaz-Emparanza and Riccardo (Jack) Lucchetti. It essentially shares similar features but does not support all frequency combinations.

You can also call the functionalities via the **GUI menu** "View -> Graph specified vars -> Season Plot".

Please ask questions and report bugs on the gretl mailing list if possible. Alternatively, create an issue ticket on the github repo (see below).
Source code and test script(s) can be found here:
https://github.com/atecon/seasonal_plot

The package comprises the three public functions `set_season_plot()`, `plot_season_plot()` and `season_plot_gui()`. The `season_plot_gui()` function is mainly a wrapper for GUI access but may be also called via scripting as a "short-cut" way for immediately showing a plot on the screen.

The `set_season_plot()` function sets all necessary information, runs some checks and computes the matrices holding the actual results. Information is stored in a bundle. The function `plot_season_plot()` calls this bundle and plots the requested result. Due to this two-step approach, the necessary computation has to be done only once.

# Public functions

```
set_season_plot(const series y, const string name_y[null])
```

Initializes various things, computes the pivoted matrices, and writes the gnuplot-files.

## Parameters

- `y`: series, Variable of interest
- `name_y`: string, Pass name of series 'y' (optional) -- only relevant for GUI wrapper and not for the user

## Returns

Bundle comprising the various items. You may be interested in the pivoted matrices stored under: `<BUNDLE_NAME>["data_to_plot"]`

---

```
do_season_plot(const bundle self, const string type, string filename[null])
```

## Parameters

- `self`: bundle, Bundle returned by `set_season_plot()` function. The user may add variables parameters for fine-tuning the plot before passing it to the `plot_season_plot()` function (see below for details, optional)
- `type`: string, String referring to the frequency component to show on the x-axis of the plot, either 'obsmajor', 'obsminor' 'obsmicro' or 'all' (for plotting all frequencies at once)
- `filename`: string, Full path and file name for storing plot (optional; if not passed, plot is shown on screen)

## Returns

No return value

## Hint

When calling `type="all"`, the user may also want to change the size of the plot. This can be done by passing the optional parameters `plot_width`, `plot_height` and `font_size` to the `do_season_plot()` function via the bundle `self`.
---


```
season_plot_gui(const series y, int frequency, const int plot_width, const int plot_height, const int font_size)
```

## Note

This function is

## Parameters

- `y`: series, Variable of interest
- `frequency`: int, Select frequency component, 1=obsmajor, 2=obsminor, 3=obsmicro (default 1)
- `plot_width`: int, control width of the plot (default 900)
- `plot_height`: int, control height of the plot (default 600)
- `font_size`: int, control font size (default 12)

## Returns

No return value. Instant plot on screen.

# Notes on frequency components

For details on frequency components, we refer to the gretl help. For instance check the help for the `$obsmajor` accessor (`<help $obsmajor>`). For most standard time-series frequencies, 'obsmajor' refers to the year, 'obsminor' to the quarter or month, and 'obsmicro' to the day of a month.

Things are different, when working with hourly data. In this case 'obsmajor' refers to the day and 'obsminor' to the hour ('obsmicro' is not defined for this frequency).

Non-standard frequencies set manually with the 'setobs' command may or may not work. In such cases please think in terms of major and minor frequency components and disregard the standard frequency labels in this package.

# Optional parameters for controlling plotting output

The user can pass the following optional parameters before calling the function `plot_seasonal_plot()`:

- `plot_width`: int (default 900) Control width of the plot (must be of integer  type)
- `plot_height`: int (default 600) Control height of the plot (must be of integer  type)
- `font_size`: int (default 12) Control font size (must be of integer  type)
- `title`: string (default "") Control title of the plot
- `cols`: int (default: automatically set) Control number of columns in the gridplot (only relevant of `type=all` is selected)
- `rows`: int (default: automatically set) Control number of rows in the gridplot (only relevant of `type=all` is selected)

# Dependencies

This package depends on the following user-contributed gretl packages:

- `string_utils` (for string manipulation; used function `strdrop()`)

# Changelog

* **v0.2 (October 2024)**
    * make use of gretl's built-in gridplot apparatus instead of using the "multiplot" package
	* rename function `plot_season_plot()` to `do_season_plot()` (**backward incompatible**)
	* rename parameters `PLOT_WIDTH` to `plot_width`, `PLOT_HEIGHT` to `plot_height` and `FONT_SIZE` to `font_size` (**backward incompatible**)
	* add new option to parameter `type` for plotting all frequencies at once (`type="all"`). This is useful for comparing the dynamics of all frequency components at once
	* add new parameter `title` for controlling the title of the plot
	* add new parameter `cols` for controlling the number of columns in the gridplot (only relevant of `type=all` is selected)
	* add new parameter `rows` for controlling the number of rows in the gridplot (only relevant of `type=all` is selected)
	* raise minimum gretl version to 2024b (due to the use of the new gridplot apparatus and especially the `--title` option)

* **v0.1 (August 2020)**
    * Initial release