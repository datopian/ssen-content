# 2024 Business Roadmap

Welcome to our comprehensive guide for our strategic business initiatives and key milestones planned for 2024. This document outlines our priorities, major project timelines, and the exciting opportunities we aim to capture.

<VegaLite
  spec={{
  "$schema": "https://vega.github.io/schema/vega/v5.json",
  "description": "a serpentine timeline. The serpentine shape can be an option for instances where an oblong canvas is not ideal. The shape can be customized using many of the signals below. Input bindings have been included for demonstration purposes",
  "padding": 15,
  "width": 300,
  "signals": [
    {
      "name": "width",
      "init": "width"
    },
    {
      "name": "sH",
      "description": "serpentine: diameter of arcs",
      "value": 125
    },
    {
      "name": "labelsOnHover",
      "description": "milestone: show labels on hover only",
      "value": false
    },
    {
      "name": "sN",
      "description": "serpentine: number of arcs",
      "value": 2.2
    },
    {
      "name": "tC",
      "description": "ticks: number of axis ticks to display on the timeline",
      "value": 21
    },
    {
      "name": "tLO",
      "description": "ticks: the offset for the tick labels",
      "value": 8
    },
    {
      "name": "mO",
      "description": "milestone: the offset for the milestone markers",
      "value": 35
    },
    {
      "name": "sR0P",
      "description": "serpentine: percentage of width of canvas for the start of the timeline",
      "value": 0
    },
    {
      "name": "sLP",
      "description": "serpentine: percentage of total length of canvas for the end of the timeline",
      "value": 1
    },
    {
      "name": "annotationStart",
      "value": ""
    },
    {
      "name": "annotationEnd",
      "value": "Today"
    },
    {
      "name": "includeArrows",
      "value": true
    },
    {
      "name": "sT",
      "description": "serpentine: thicknes of the line",
      "value": 5
    },
    {
      "name": "domain",
      "init": "[year(now())-100, year(now())]",
      "description": "serpentine: manually set the domain extent for the timeline, otherwise set to null to have the domain calculated for you"
    },
    {
      "name": "sRange",
      "description": "serpentine: range for the serpentine scale",
      "update": "[sR0P*width,sL*sLP]"
    },
    {
      "name": "annotations",
      "description": "serpentine: annotations that appear at the start and end of the timeline",
      "update": "{start: (isValid(annotationStart) ? annotationStart : ''), end: (isValid(annotationEnd) ? annotationEnd : '')}"
    },
    {
      "name": "sD",
      "update": "[2, 2]",
      "description": "serpentine: dash array for the serpentine line"
    },
    {
      "name": "reverse",
      "description": "serpentine: boolean to indicate whether the scale for the timeline should be reversed",
      "value": false
    },
    {
      "name": "sPct",
      "description": "serpentine: percentage of width for the straight portions of the timeline",
      "value": 1,
      "update": "sPct < 0.25 ? 0 : sPct < 0.75 ? 0.5 : 1"
    },
    {
      "name": "sW",
      "description": "serpentine: horizontal length of straight segments",
      "update": "sPct*width"
    },
    {
      "name": "sL",
      "description": "serpentine: total length of line",
      "update": "(sN+1)*sW+(sN)*sH*PI/2"
    },
    {
      "name": "sA",
      "description": "serpentine: length of an arc segment",
      "update": "(sH*PI/2)"
    },
    {
      "name": "sWsA",
      "description": "serpentine: length of a line + arc segment",
      "update": "(sW + sH*PI/2)"
    },
    {
      "name": "sDomain",
      "description": "serpentine: domain for the serpentine scale",
      "init": "domain ? domain : [+extent(pluck(data('dataset'), 'domain'))[0], +extent(pluck(data('dataset'), 'domain'))[1]]"
    },
    {
      "name": "hoverFocus",
      "value": 0
    },
    {
      "name": "height",
      "description": "calculated height",
      "update": "extent(pluck(data('serpentine'), 'y'))[1]"
    }
  ],
  "scales": [
    {
      "name": "sS1",
      "type": "linear",
      "zero": false,
      "reverse": { "signal": "reverse" },
      "domain": { "signal": "sDomain" },
      "range": { "signal": "sRange" }
    },
    {
      "name": "footerY",
      "type": "band",
      "domain": { "data": "footer", "field": "id" },
      "range": [
        { "signal": "height+60" },
        { "signal": "height+60+length(data('footer'))*11" }
      ]
    }
  ],
  "marks": [
    {
      "name": "axis_group",
      "description": "group containing all the axis marks - annotations, domain, arrow indicators, tick lines, tick labels on straightaways, tick labels on arcs",
      "type": "group",
      "marks": [
        {
          "name": "annotations",
          "description": "Text marks that appear at the start and end of the timeline. Configured using the 'annotations' signal",
          "from": { "data": "domain_extent" },
          "type": "text",
          "interactive": false,
          "encode": {
            "update": {
              "x": { "field": "x" },
              "y": { "field": "y" },
              "text": {
                "signal": "datum['category'] === 'start' ? annotations['start'] : annotations['end']"
              },
              "fontSize": { "value": 10 },
              "baseline": { "value": "middle" },
              "align": { "field": "align" },
              "angle": { "field": "angle" },
              "dx": { "field": "dx" },
              "dy": { "signal": "1" },
              "fill": { "value": "gray" }
            }
          }
        },
        {
          "name": "serpentine_line",
          "description": "The serpentine-shaped line that acts as the axis domain line",
          "type": "line",
          "from": { "data": "serpentine" },
          "interactive": false,
          "encode": {
            "update": {
              "x": { "field": "x" },
              "y": { "field": "y" },
              "strokeDash": { "signal": "sD" },
              "stroke": { "value": "#888" },
              "strokeWidth": { "signal": "sT" }
            }
          }
        },
        {
          "name": "arrow_marks",
          "description": "The arrows to indicate direction that appear at the beginning and end of each arc+straightaway combonation",
          "type": "text",
          "from": { "data": "segment_ends" },
          "interactive": false,
          "encode": {
            "update": {
              "x": { "field": "x" },
              "y": { "field": "y" },
              "dy": { "value": 1 },
              "text": { "value": "➤" },
              "fontSize": { "signal": "18" },
              "fill": { "value": "#000" },
              "stroke": { "value": "#fff" },
              "strokeWidth": { "value": 1 },
              "angle": { "signal": "datum['direction'] === '→' ? 0 : 180" },
              "align": { "value": "center" },
              "baseline": { "value": "middle" }
            }
          }
        },
        {
          "name": "tick_marks",
          "description": "The line (text mark) designated to each tick",
          "from": { "data": "ticks" },
          "type": "text",
          "interactive": false,
          "encode": {
            "update": {
              "x": { "field": "x" },
              "y": { "field": "y" },
              "dy": {
                "signal": "2.5*( datum['type'] === 'straight' ? 1 : datum['side'] === 'right' ? (round(datum['alpha']*(180/PI)) >= 90 ? -1 : 4) : (round(datum['alpha']*(180/PI)) > 89 ? -1 : 4))"
              },
              "text": { "signal": "'|'" },
              "fontSize": { "signal": "7" },
              "fill": { "value": "#000" },
              "angle": { "field": "labelAngle" },
              "align": { "value": "center" },
              "baseline": {
                "signal": "datum['type'] === 'straight' ? 'top' : 'bottom'"
              }
            }
          }
        },
        {
          "name": "tick_labels_straight",
          "description": "The straightaway tick labels",
          "from": { "data": "ticks" },
          "type": "text",
          "interactive": false,
          "encode": {
            "update": {
              "x": { "field": "x" },
              "y": { "field": "y" },
              "dy": { "field": "dy" },
              "text": { "field": "domain" },
              "fontSize": { "signal": "10" },
              "fill": { "value": "#000" },
              "align": { "value": "center" },
              "angle": { "field": "labelAngle" },
              "baseline": {
                "signal": "datum['type'] === 'straight' ? 'top' : 'bottom'"
              }
            }
          }
        }
      ]
    },
    {
      "name": "milestone_connecting_lines",
      "description": "The milestone lines that connect the markers to labels",
      "from": { "data": "milestones" },
      "type": "text",
      "interactive": false,
      "encode": {
        "update": {
          "text": { "signal": "'|'" },
          "x": { "field": "x" },
          "y": { "field": "y" },
          "fontSize": { "value": 15 },
          "fontWeight": { "value": 100 },
          "dy": {
            "signal": "datum['type'] === 'arc' && (round(datum['alpha']*(180/PI)) > 89) ? 0.35 * (mO+3.5) : datum['dy']/2"
          },
          "align": { "value": "center" },
          "baseline": { "value": "middle" },
          "angle": { "field": "labelAngle" },
          "fillOpacity": { "value": 0.35 },
          "opacity": {
            "signal": "isValid(hoverFocus) && datum['label'] === hoverFocus['label'] ? 1 : labelsOnHover ? 0 : 1"
          }
        }
      }
    },
    {
      "name": "milestone_markers",
      "description": "The milestone timeline markers",
      "from": { "data": "milestones" },
      "type": "symbol",
      "interactive": false,
      "encode": {
        "update": {
          "x": { "field": "x" },
          "y": { "field": "y" },
          "size": {
            "signal": "labelsOnHover && isValid(hoverFocus) && datum['label'] === hoverFocus['label'] ? 200 : 150"
          },
          "fill": {
            "signal": "labelsOnHover ? isValid(hoverFocus) && datum['label'] === hoverFocus['label'] ? 'firebrick' : '#e4b3b4' : 'firebrick'"
          },
          "stroke": {
            "signal": "labelsOnHover ? isValid(hoverFocus) && datum['label'] === hoverFocus['label'] ? '#fff' : 'firebrick' : '#fff'"
          },
          "strokeWidth": { "signal": "labelsOnHover ? 1 : 2" },
          "cursor": { "signal": "labelsOnHover ? 'pointer' : 'default'" }
        }
      }
    },
    {
      "name": "milestone_label_backgrounds",
      "description": "The white backgrounds for milestone labels",
      "from": { "data": "milestones" },
      "type": "text",
      "interactive": false,
      "encode": {
        "update": {
          "x": { "field": "x" },
          "y": { "field": "y" },
          "dy": { "field": "dy" },
          "text": { "signal": "datum['domain'] + ' - ' + datum['label']" },
          "fontSize": { "signal": "10" },
          "fill": { "value": "#fff" },
          "stroke": { "value": "#fff" },
          "strokeWidth": { "value": 7 },
          "angle": { "field": "labelAngle" },
          "align": { "value": "center" },
          "baseline": {
            "signal": "datum['type'] === 'straight' ? 'top' : 'bottom'"
          },
          "opacity": {
            "signal": "isValid(hoverFocus) && datum['label'] === hoverFocus['label'] ? 1 : labelsOnHover ? 0 : 1"
          }
        }
      }
    },
    {
      "name": "milestone_labels",
      "description": "The milestone labels",
      "from": { "data": "milestones" },
      "type": "text",
      "interactive": false,
      "encode": {
        "update": {
          "x": { "field": "x" },
          "y": { "field": "y" },
          "dy": { "field": "dy" },
          "text": { "signal": "datum['domain'] + ' - ' + datum['label']" },
          "fontSize": { "signal": "10" },
          "fill": { "value": "firebrick" },
          "angle": { "field": "labelAngle" },
          "align": { "value": "center" },
          "baseline": {
            "signal": "datum['type'] === 'straight' ? 'top' : 'bottom'"
          },
          "opacity": {
            "signal": "isValid(hoverFocus) && datum['label'] === hoverFocus['label'] ? 1 : labelsOnHover ? 0 : 1"
          }
        }
      }
    }
  ],
  "data": [
    {
      "name": "dataset",
      "values": [
        { "domain": 1928, "label": "Major Event A" },
        { "domain": 1939, "label": "Major Event B" },
        { "domain": 1954, "label": "Major Event C" },
        { "domain": 1962, "label": "Major Event D" },
        { "domain": 1974, "label": "Major Event E" },
        { "domain": 1990, "label": "Major Event F" },
        { "domain": 2001, "label": "Major Event G" },
        { "domain": 2015, "label": "Major Event H" }
      ]
    },
    {
      "name": "serpentineDomain",
      "values": [{}],
      "transform": [
        {
          "type": "formula",
          "expr": "sequence(sDomain[0],sDomain[1], 0.1 )",
          "as": "domain"
        },
        { "type": "flatten", "fields": ["domain"] }
      ]
    },
    {
      "name": "milestoneDomain",
      "source": "dataset",
      "transform": [{ "type": "project", "fields": ["domain"] }]
    },
    {
      "name": "tickDomain",
      "values": [{}],
      "transform": [
        { "type": "formula", "expr": "sequence(1,tC+1, 1)", "as": "id" },
        { "type": "flatten", "fields": ["id"] },
        {
          "type": "formula",
          "expr": "datum['id'] === 1 ? sDomain[0] : datum['id'] === tC ? sDomain[1] : null",
          "as": "domain"
        },
        {
          "type": "formula",
          "expr": "round(isValid(datum['domain']) ? datum['domain'] : (sDomain[0] + (sDomain[1]-sDomain[0])*((datum['id']-1)/(tC-1))))",
          "as": "domain"
        },
        { "type": "project", "fields": ["domain"] }
      ]
    },
    {
      "name": "componentEncodings",
      "values": [
        { "category": "start" },
        { "category": "serpentine" },
        { "category": "milestone" },
        { "category": "tick" },
        { "category": "end" }
      ],
      "transform": [
        { "type": "formula", "expr": "now()", "as": "timestamp" },
        {
          "type": "formula",
          "expr": "datum['category'] === 'start' ? [sDomain[reverse ? 1 : 0]] : datum['category'] === 'serpentine' ? pluck(data('serpentineDomain'), 'domain') : datum['category'] === 'milestone' ? pluck(data('dataset'), 'domain') : datum['category']==='tick' ? pluck(data('tickDomain'), 'domain') : datum['category'] === 'end' ? [sDomain[reverse ? 0 : 1]] : null",
          "as": "domain"
        },
        { "type": "flatten", "fields": ["domain"] },
        { "type": "formula", "expr": "+datum['domain']", "as": "domain" },
        {
          "type": "window",
          "ops": ["row_number"],
          "sort": { "field": "domain" },
          "groupby": ["category"],
          "as": ["id"]
        },
        {
          "type": "formula",
          "expr": "scale('sS1', datum['domain'])",
          "as": "sK"
        },
        {
          "type": "formula",
          "expr": "floor(datum['sK'] / (sW + sH*PI/2))",
          "as": "i"
        },
        {
          "type": "formula",
          "expr": "datum['sK'] % (sW + sH*PI/2)",
          "as": "r"
        },
        {
          "type": "formula",
          "expr": "(datum['r'] - sW)/(sH/2)",
          "as": "alpha"
        },
        {
          "type": "formula",
          "expr": "(((datum['i']+1)*sWsA)-sA) >= datum['sK'] ? 'straight' : 'arc'",
          "as": "type"
        },
        {
          "type": "formula",
          "expr": "(datum['i']%2 == 0) ? min(datum['r'],sW) : max(sW-datum['r'], 0)",
          "as": "xStraight"
        },
        {
          "type": "formula",
          "expr": "datum['type'] === 'straight' ? datum['xStraight'] : datum['xStraight'] + (datum['i']%2 == 0 ? sin(datum['alpha'])*sH/2 : -sin(datum['alpha'])*sH/2)",
          "as": "x"
        },
        {
          "type": "formula",
          "expr": "datum['type'] === 'straight' ? datum['i']*sH : (datum['i']*sH) + (1 - cos(datum['alpha']))*sH/2",
          "as": "y"
        },
        {
          "type": "formula",
          "expr": "datum['type'] === 'straight' ? null : datum['i']%2===0?'right':'left'",
          "as": "side"
        },
        {
          "type": "formula",
          "expr": "datum['type'] === 'straight' ? null : datum['alpha']<PI/2?'top':'bottom'",
          "as": "hemisphere"
        },
        {
          "type": "formula",
          "expr": "datum['type'] === 'straight' ? 0 : (datum['side'] === 'left' ? -1 : 1) *datum['alpha']*(180/PI)+(datum['alpha'] < PI/2 ? 0 : 180)",
          "as": "labelAngle"
        },
        {
          "type": "formula",
          "expr": "datum['i']%2===0 ?'→':'←'",
          "as": "direction"
        },
        {
          "type": "formula",
          "expr": "datum['type'] === 'straight' ? datum['direction'] : datum['side'] === 'left' ? datum['hemisphere'] === 'top'? '←' : '→' :  datum['hemisphere'] === 'top'? '→' : '←'",
          "as": "direction"
        }
      ]
    },
    {
      "name": "serpentine",
      "source": "componentEncodings",
      "transform": [
        { "type": "filter", "expr": "datum['category'] === 'serpentine'" }
      ]
    },
    {
      "name": "ticks",
      "source": "componentEncodings",
      "transform": [
        { "type": "filter", "expr": "datum['category'] === 'tick'" },
        {
          "type": "formula",
          "expr": "!isValid(datum['side']) ? (tLO+3.5) : datum['side'] === 'right' ? (round(datum['alpha']*(180/PI)) >= 90 ? -1 : 1.75) * (tLO+3.5) : (round(datum['alpha']*(180/PI)) > 89 ? -1 : 1.75) * (tLO+3.5)",
          "as": "dy"
        }
      ]
    },
    {
      "name": "domain_extent",
      "source": "componentEncodings",
      "transform": [
        {
          "type": "filter",
          "expr": "datum['category'] === 'start' || datum['category'] === 'end'"
        },
        {
          "type": "formula",
          "expr": "datum['category'] === 'start' ? 'right' : datum['direction'] === '←' && datum['type'] === 'straight' ? 'right' : datum['side'] === 'left' ? 'right' : 'left'",
          "as": "align"
        },
        {
          "type": "formula",
          "expr": "datum['category'] === 'start' ? -tLO-5 :  datum['direction'] === '←'  && datum['type'] === 'straight' ? -tLO-5 : datum['side'] === 'left' ? -tLO-5 : tLO+5",
          "as": "dx"
        }
      ]
    },
    {
      "name": "segment_ends",
      "source": "componentEncodings",
      "transform": [
        {
          "type": "filter",
          "expr": "includeArrows && datum['category'] === 'serpentine'"
        },
        {
          "type": "joinaggregate",
          "fields": ["id", "id"],
          "ops": ["min", "max"],
          "groupby": ["i"],
          "as": ["minId", "maxId"]
        },
        {
          "type": "filter",
          "expr": "datum['id'] === (reverse ? datum['maxId'] : datum['minId'])"
        }
      ]
    },
    {
      "name": "footer",
      "values": [
        {
          "id": 1,
          "text": [
            "Data Source:",
            "https://ourworldindata.org/mass-extinctions"
          ],
          "href": "https://ourworldindata.org/mass-extinctions"
        },
        {
          "id": 2,
          "text": ["Icons:", "https://fontawesome.com/"],
          "href": "https://fontawesome.com/"
        },
        {
          "id": 3,
          "text": ["Data Viz By:", "Madison Giammaria"],
          "href": "https://www.linkedin.com/in/madison-giammaria-58463b33"
        }
      ]
    },
    {
      "name": "milestones",
      "source": "componentEncodings",
      "transform": [
        { "type": "filter", "expr": "datum['category'] === 'milestone'" },
        {
          "type": "lookup",
          "key": "domain",
          "from": "dataset",
          "fields": ["domain"],
          "values": ["label", "color"]
        },
        {
          "type": "formula",
          "expr": "!isValid(datum['side']) ? -(mO) : datum['side'] === 'right' ? (round(datum['alpha']*(180/PI)) >= 90 ? 1 : -0.5) * (mO+3.5) : (round(datum['alpha']*(180/PI)) > 89 ? 1 : -0.5) * (mO+3.5)",
          "as": "dy"
        },
        {
          "type": "formula",
          "expr": "(isValid(hoverFocus) && hoverFocus['domain']===datum['domain']) ? 1 : 0.4",
          "as": "fillOpacity"
        }
      ]
    }
  ]
}}
/>

## Executive Summary

The SSEN Data Roadmap serves as the strategic plan outlining the key milestones related to data provision and management. It provides a structured path for implementing data sets and achieving data strategy goals.

> 🌟 **NOTE**
> 
> This roadmap is a living document and may evolve based on market and data consumer feedback and internal strategy shifts.

## Purpose of Roadmap

**The SSEN Data Roadmap serves as the strategic plan outlining the key milestones related to data provision and management. It provides a structured path for implementing data sets and achieving data strategy goals.**

The Data Roadmap promotes open collaboration and transparent data sharing with stakeholders. By outlining clear milestones and objectives, it creates a shared vision with stakeholders which encourages active involvement and feedback, where insights and requirements can be openly addressed. This ensures stakeholders are well informed about the data journey, promoting trust and cooperation in achieving common data-related goals.

## Who is it for?

**The SSEN Data Portal and Data Roadmap is open to everyone, including:**

- Academia & Research Bodies
- Battery Storage Owners
- Commercial Aggregator
- Community Aggregator
- Consultancies
- Consumer Advocacy Groups
- Critical National Infrastructure
- Distributed Generators
- Distrubution Network Operators
- Domestic Aggregator
- Electricity Storage Providers
- Electrical System Operator 
- Gas Distrubution Networks
- Industrial & Commercial Customers
- Internal Stakeholders
- Local Government/Authorities
- Regulatory Bodies
- Supplier Aggregator
- Tranmission Operators 
- Trade Associations
- Flexibility Service Providers 

## Some Use Cases for the Data Portal and Roadmap

- Understanding how to connect, where and how much it would cost through [New electricity supplies](https://github.com/datopian/ssen-content/assets/20338818/fd3da564-4f9d-4df4-a280-9c976ef45581)
- Visibility of near real time consumption in an area
- Information on past and future outages
- Monetising assets from flexibility services through [Flexible Solutions](https://www.ssen.co.uk/our-services/flexible-solutions/)
- Future energy predictions, scenarios and plans for a local area through [Local Area Energy Planner Plus](https://www.ssen.co.uk/about-ssen/dso/whole-system/local-area-energy-planning/)
- Technical information to help with modelling
- Research and collaborating with other stakeholders


## How did we come up with the roadmap?

**The Data Roadmap was created looking at 5 main areas:**

- Retrospective – what existing data does SSEN have that stakeholders would benefit from?
- Prospective – what data is SSEN going to produce in the near term  that stakeholders would benefit from?
- Stakeholder requests/feedback – obtained through direct requests via the data portal as well as stakeholder feedback from surveys, roundtables, etc 
- Legal requirements – e.g., from a regulatory standpoint 
- Future looking – what are customers or partners asking for in the future?

> 💡 **TIP**
> 
>  You can input into our data roadmap through our [request form](https://forms.office.com/e/tKYxkTWS0n) or emailing us data@ssen.co.uk 

## How will we continue to engage stakeholders moving forward?

- Regular Updates – provide consistent and transparent updates on the progress of the data roadmap. Visualised reports will be used to present data roadmap progress, what has been published and what is to come 
- Feedback mechanisms– we will continue to collect stakeholder feedback from our feedback form on the data portal as well as through stakeholder surveys and other dedicated channels. We will act on the feedback in a  timely manner to demonstrate responsiveness and adaptability 
- Stakeholder Forums – we are creaating forums and discussion groups on the data portal where stakeholders can share their perspectives and ideas on the data roadmap and what they would like to see included, fostering a sense of shared ownership
- Providing tailored information to stakeholders-  use cases and the target audience for all data sets are included with the data sets based on interest and relevance to various stakeholder groups 
- Accessibility  – ensure that information about the data roadmap is easily accessible to all stakeholders to support understanding and engagement through the use of appropriate documentation e.g video demonstrations, etc
- Demonstrate value – highlight the value generated from completed phases of the roadmap, showing tangible benefits e.g improved data accessibility, etc which will be shown through our regular updates on our Digital Action Plan.

## How does the roadmap link to the Digital Strategy and Action Plan?

As mentioned in our [Digital Strategy](https://www.ssen.co.uk/about-ssen/digital-strategy/our-digital-strategy/), our role in the future energy system is driven by Digital and Data. The move to a flexible, decentralised energy system would not be possible without making our systems  and processes more digital and improving our ability to use and share data.  Hence, our Data portal and Data Roadmap are imperative. We have created the Data Roadmap as a forward looking view of our data as we aim to share data in an open and transparent way and want to gain feedback on what's to come as we want to ensure our portal has the right data our stakeholders want, to a quality they want, and ensure we are improving how we think about and handle data within our business.

Our Digital strategy sits alongside our [Digital Action Plan](https://www.ssen.co.uk/about-ssen/digital-strategy/our-digital-action-plan/), which we update every 
6 months and gives you the detail on when we will deliver our products and services and how we will be measured on their performance. Therefore, you will be able to see regular updates on how we are delivering our Data Portal and Data Roadmap as well as the value they have generated.

## What if I need additional support and help? 

For customers who may require additional assistance, please access the Recite Me Web Accessibility and Language Toolbar on our website: 

www.ssen.co.uk/about-ssen/customer-support/digital-accessibility-on-reciteme/  

Your satisfaction is important to us and we are committed to providing the support that you need to make the most out of our services.
