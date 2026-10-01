# Web figures in React

## plotly.js

- For standard charts, use `react-plotly.js`.
- For more than about 10,000 points, use the WebGL traces (`scattergl`).

## D3

- Let React own the DOM. Use the D3 modules for the calculations (`d3-scale`, `d3-shape`, `d3-array`, `d3-hierarchy`), and render the SVG in JSX.
- Use `d3-selection` in a `useRef` hook only for behavior that React cannot do: zoom, brush, drag, and transitions.
- Join the data by a stable key, not by the array index.
- Make the figure responsive with a `viewBox`, and use a `ResizeObserver` to get the width.

## Data and access

- Follow the layer rules in `rules/python.md`. The API sends the data in the shape that the chart uses. The frontend does not calculate statistics.
- Aggregate large data on the server, with DuckDB or PostgreSQL. Do not send raw rows that the reader cannot see.
- Give each SVG a `<title>` and a `role="img"` with an `aria-label`. Give an interactive figure keyboard access and a data table as an alternative.
