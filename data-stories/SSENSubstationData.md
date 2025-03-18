# SSEN Substation Data

SSEN Distribution has published it's Substation data.

We are going to consider the following resource from our Dev environment:

https://portal-dev.sse.datopian.com/@ssen-distribution/substation-data-test/r/a15a77d1-2d31-4d80-9a96-9a594911cd2a

⚠️ You need to be logged in to see the page above.

Below is the preview of the data above:

TODO

⚠️ Note that I took URL from Download button on the resource page to use for the component above.

<Vega spec={{
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "description": "A configurable map of countries of the world.",
  "width": 900,
  "height": 500,
  "autosize": "none",

  "signals": [
    {
      "name": "type",
      "value": "mercator",
      "bind": {
        "input": "select",
        "options": [
          "albers",
          "albersUsa",
          "azimuthalEqualArea",
          "azimuthalEquidistant",
          "conicConformal",
          "conicEqualArea",
          "conicEquidistant",
          "equalEarth",
          "equirectangular",
          "gnomonic",
          "mercator",
          "naturalEarth1",
          "orthographic",
          "stereographic",
          "transverseMercator"
        ]
      }
    },
    { "name": "scale", "value": 150,
      "bind": {"input": "range", "min": 50, "max": 2000, "step": 1} },
    { "name": "rotate0", "value": 0,
      "bind": {"input": "range", "min": -180, "max": 180, "step": 1} },
    { "name": "rotate1", "value": 0,
      "bind": {"input": "range", "min": -90, "max": 90, "step": 1} },
    { "name": "rotate2", "value": 0,
      "bind": {"input": "range", "min": -180, "max": 180, "step": 1} },
    { "name": "center0", "value": 0,
      "bind": {"input": "range", "min": -180, "max": 180, "step": 1} },
    { "name": "center1", "value": 0,
      "bind": {"input": "range", "min": -90, "max": 90, "step": 1} },
    { "name": "translate0", "update": "width / 2" },
    { "name": "translate1", "update": "height / 2" },

    { "name": "graticuleDash", "value": 0,
      "bind": {"input": "radio", "options": [0, 3, 5, 10]} },
    { "name": "borderWidth", "value": 1,
      "bind": {"input": "text"} },
    { "name": "background", "value": "#ffffff",
      "bind": {"input": "color"} },
    { "name": "invert", "value": false,
      "bind": {"input": "checkbox"} }
  ],

  "projections": [
    {
      "name": "projection",
      "type": {"signal": "type"},
      "scale": {"signal": "scale"},
      "rotate": [
        {"signal": "rotate0"},
        {"signal": "rotate1"},
        {"signal": "rotate2"}
      ],
      "center": [
        {"signal": "center0"},
        {"signal": "center1"}
      ],
      "translate": [
        {"signal": "translate0"},
        {"signal": "translate1"}
      ]
    }
  ],

  "data": [
    {
      "name": "world",
      "url": "https://raw.githubusercontent.com/vega/vega-datasets/main/data/world-110m.json",
      "format": {
        "type": "topojson",
        "feature": "countries"
      }
    },
    {
      "name": "graticule",
      "transform": [
        { "type": "graticule" }
      ]
    }
  ],

  "marks": [
    {
      "type": "shape",
      "from": {"data": "graticule"},
      "encode": {
        "update": {
          "strokeWidth": {"value": 1},
          "strokeDash": {"signal": "[+graticuleDash, +graticuleDash]"},
          "stroke": {"signal": "invert ? '#444' : '#ddd'"},
          "fill": {"value": null}
        }
      },
      "transform": [
        { "type": "geoshape", "projection": "projection" }
      ]
    },
    {
      "type": "shape",
      "from": {"data": "world"},
      "encode": {
        "update": {
          "strokeWidth": {"signal": "+borderWidth"},
          "stroke": {"signal": "invert ? '#777' : '#bbb'"},
          "fill": {"signal": "invert ? '#fff' : '#000'"},
          "zindex": {"value": 0}
        },
        "hover": {
          "strokeWidth": {"signal": "+borderWidth + 1"},
          "stroke": {"value": "firebrick"},
          "zindex": {"value": 1}
        }
      },
      "transform": [
        { "type": "geoshape", "projection": "projection" }
      ]
    }
  ]
}}
/>

## Testing FlatUI

<FlatUiTable 
  url="https://storage.openspending.org/alberta-budget/__os_imported__alberta_total.csv" 
/>
