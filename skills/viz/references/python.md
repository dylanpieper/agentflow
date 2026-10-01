# Python figures and tables

- For static figures, use `plotnine`. It has the same grammar as ggplot2. Use `seaborn` for a quick statistical plot, and `matplotlib` only for fine control of a figure.
- For distributions, show the points with `seaborn.stripplot()` or `seaborn.swarmplot()`. Put them on a narrow `seaborn.boxplot(showfliers=False)`.
- For color, use `seaborn.color_palette("colorblind")` or a viridis palette.
- For interactive charts, use `plotly.express`. For more than about 10,000 points, use the WebGL traces (`render_mode="webgl"`).
- For tables, use `great_tables`. It has the same grammar as gt.
