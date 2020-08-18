# season_plot

Plot seasonal components of a time-series by frequency groups.

This package computes and depicts a time-series across sub-periods, also
called a Buys-Ballot plot.

For instance, if one has a quarterly time-series one can plot the dynamics of each quarter across years. Vice versa, you may also be interested in plotting the dynamics of each year's quarter.

Note, you may also try the user-written gretl package ```buys_ballot``` written by Ignacio Diaz-Emparanza and Riccardo (Jack) Lucchetti. It essentially shares similar features but does not support all frequency combinations.

# Installation and usage

Get the package from the gretl package server and install it:
```
pkg install season_plot
```

Here is a sample script on how to use it (see also: https://raw.githubusercontent.com/atecon/season_plot/master/src/season_plot_sample.inp):

```
clear
set verbose off
open denmark -q

include season_plot.gfn

open data9-13.gdt -p -q  # monthly
series y = bkret

# prepare bundle object
bundle b = set_season_plot(y)

# Start plotting for the "obsmajor" (here year) frequency component
plot_season_plot(b, "obsmajor", "display")
```

See that we've passed as the 2nd argument the string "obsmajor" which refers to the lowest frequency comonent (year) the monthly time-series data set. The resulting figure shows the dynamics of each month between the years 1990 and 1998:

![sample](https://github.com/atecon/season_plot/raw/master/plot1.png)

Dependent on the frequency of the time-series, gretl distinguishes between the three fruency components "obsmajor" (typically 'year'), "obsminor" (typically 'month' or quarter) and obsmicro (typically 'day'). If


# Publich functions

The package comprises the three public functions set_season_plot(), plot_season_plot() and season_plot_gui(). The season_plot_gui() function is mainly a wrapper for GUI access but may be also called via scripting as a "short-cut" way for immediately showing a plot on the screen.

The set_season_plot() function sets all necessary information, runs some checks and computes the matrices holding the actual results. Informations is stored in a bundle. The function plot_season_plot() calls this bundle and plots the requested result. Due to this two-step approach, the necessary computation has to be done only once.


Function:       *set_season_plot(const series y, const string name_y[null])*

This function initializes various things and computes the pivoted matrices.

Arguments:
- ```y```:    series, Variable of interest
- ```name_y```:    string, Pass name of series 'y' (optional) -- only relevant for GUI wrapper and not for the user.

Return: *Bundle comprising the various items. You may be interested in the pivoted matrices stored under: <BUNDLE_NAME>["data_to_plot"]*
-------------------


Function:       *plot_season_plot (const bundle self, const string type, string filename[null])*

This function calls the actual plotting facility.

Arguments:
- ```self```:		bundle, Bundle returned by set_season_plot() function. The
	               	user may add variables parameters for fine-tuning the plot
					before passing it to the plot_season_plot() function (see
					below for details, optional)
- ```type```:   	string, String referring to the frequency component to show
	               	on the x-axis of the plot, either 'obsmajor', 'obsminor'
	               	or 'obsmicro'.
- ```filename```: 	string, Full path and file name for storing plot.

Return: *No return value*
---------------------


Function:       *season_plot_gui (const series y, int frequency, const int PLOT_WIDTH, const int PLOT_HEIGHT, const int FONT_SIZE)*

This function is supposed to be for GUI access only.

Arguments:
- ```y```	:		series, Variable of interest
- ```frequency```:	int, Select frequency component, 1=obsmajor, 2=obsminor, 3=obsmicro (default 1)
- ```PLOT_WIDTH```: int, control width of the plot (default 900)
- ```PLOT_HEIGHT```: int, control height of the plot (default 600)
- ```FONT_SIZE```:   int, control font size (default 12)

Return: *No return value. Instant plot on screen.*
---------------------

## Notes on frequency components

For details on frequency components, we refer to the gretl help. For instance check the help for the ```$obsmajor``` accessor (<help $obsmajor>). For most standard time-series frequencies, 'obsmajor' refers to the year, 'obsminor' to the quarter or month, and 'obsmicro' to the day of a month.

Things are different, when working with hourly data. In this case 'obsmajor' refers to the day and 'obsminor' to the hour ('obsmicro' is not defined for this frequency).

Non-standard frequencies set manually with the 'setobs' command may or may not work. In such cases please think in terms of major and minor frequency components and disregard the standard frequency labels in this package.

## The optional parameters for controlling multiplot output

The user can pass the following optional parameters before calling the function ```plot_seasonal_plot()```:

- PLOT_WIDTH:        int, Control width of the plot (default 900)
- PLOT_HEIGHT:       int, Control height of the plot (default 600)
- FONT_SIZE:         int, Control font site (default 12)


# Changelog
- v0.1, August 2020:
    + initial release

