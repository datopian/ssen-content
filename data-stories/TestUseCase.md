---
title: Energy Consumption Visualization
---

This visualization shows energy consumption over time by region.

<VegaLite
    data={{
      "name": "table",
      "url": "https://data-api.ssen.co.uk/api/3/action/datastore_search?resource_id=3c999060-8c4c-4c23-b74b-ca6ef8f09297",
      "format": {"type": "json", "property": "result.records"}
    }}
    spec={{
        "$schema": "https://vega.github.io/schema/vega-lite/v5.json",
        "description": "A simple line chart showing energy consumption over time by region.",
        "data": {
          "name": "table"
        },
        "mark": "line",
        "encoding": {
          "x": {"field": "TimePeriod", "type": "temporal"},
          "y": {"field": "EnergyConsumption", "type": "quantitative"},
          "color": {"field": "Region", "type": "nominal"}
        }
      }}
/>;
