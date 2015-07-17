---
title: DCAT/data.json fields
layout: "default"
---

## Introduction

This page describes the technical detail of data.gov.uk's support of DCAT and data.json. DCAT and data.json are metadata formats suitable for harvesting into data.gov.uk. More about this is described here: [Harvesting Data into data.gov.uk - DCAT](harvesting.html#dcat)

## Dataset

| data.json field | DCAT predicate | Example value | Comments |
| --------------- | -------------- | ------------- | -------- |
| title | dct:title <br> rdfs:label | Spend over £500 | Mandatory |
| description | dct:description <br> rdfs:comment | Spend transactions published monthly according to the Treasury transparency guidelines. | Mandatory |
| identifier | (equivalent to RDF object's URI or) dct:identifier | `https://data.some.org/catalog/datasets/9df8df51-63db-37a8-e044-0003ba9b0d98` or `http://dx.doi.org/10.7927/H4PZ56R2` | Mandatory. Must stay the same, even if the dataset's title changes. Must be globally unique - not just unique to the publisher. A URI is highly recommended (ideally one that returns a web page about the dataset). |
| license | dct:license | `http://www.nationalarchives.gov.uk/doc/open-government-licence/version/2/` | Mandatory. Either a license URI or a title (must be exact or may not be recognized as open) |
| keyword | dcat:keyword | data.json: <br> `"keyword": {"geochemistry", "geology"}` <br> DCAT: `<dcat:keyword>geochemistry</dcat:keyword>` `<dcat:keyword>geology</dcat:keyword>` | Not displayed by data.gov.uk but helps theme categorization |
| issued | dct:issued | `2012-05` | Date that the data was first published. Formats allowed are: 'YYYY-MM-DD', 'YYYY-MM', 'YYYY' or 'YYYY-MM-DDTHH:MM:SS.mmmmmm' (according to ISO8601) |
| modified | dct:modified | `2012-06-10` | Date that the data was most recently updated. Formats allowed are: 'YYYY-MM-DD', 'YYYY-MM', 'YYYY' or 'YYYY-MM-DDTHH:MM:SS.mmmmmm' (according to ISO8601) |
| publisher | dct:publisher | data.json: <br> `"publisher": {"name": "Geological Society", "mbox": "info@gs.org"}` <br> DCAT: <br> `<dct:publisher>`<br>`    <foaf:Organization>`<br>`        <foaf:name>Geological Society</foaf:name>`<br>`        <foaf:mbox>info@gs.org </foaf:mbox>`<br>`    </foaf:Organization>`<br>`</dct:publisher>`| Mandatory. However the harvester will attach the dataset to the publisher selected in the harvest source. |
| distribution | dcat:distribution || A location for accessing/obtaining the data. See "Distribution" table below |
| landingPage | dcat:landingPage | `https://www.gov.uk/government/statistical-data-sets/oil-and-petroleum-products-weekly-statistics` | URL of a web page specifically about this dataset and probably has direct links to its resources and further information. |
| references | foaf:Document | `http://data.dft.gov.uk/road-accidents-safety-data/Brief-guide-to%20road-accidents-and-safety-data.doc` <br> OR in data.json you can specify more fields like a distribution e.g. <br> `{"url": "https://indicators.ic.nhs.uk/framework.pdf", "title": "Framework specification", "format": "application/pdf"}` | URL of a web page, PDF or other documentation that goves more information about the dataset. (Although use landingPage instead for a URL if that is more appropriate.) Can be multiple URLs. |
| language | dct:language | `en` or `http://id.loc.gov/vocabulary/iso639-1/en` | Language of the data |
| frequency | dct:accrualPeriodicity | `R/P1Y` (=annual) <br> `R/P1W` (=weekly) | The frequency at which dataset is published. Format: ISO 8601 Repeating Duration (or `irregular`) See: <https://project-open-data.cio.gov/iso8601_guidance/#accrualperiodicity> |
| temporal | dct:temporal | `2000-01-15/2010-01-15` | The date period that the data applies to. Formatted as two ISO 8601 dates (or datetimes) separated by a slash. |
| spatial |  dct:spatial | `{\"type\":\"Polygon\",\"coordinates\":[[[2.072, 49.943],[2.072, 55.816], [-6.236, 55.816], [-6.236, 49.943], [2.072, 49.943]]]}` | The geographic location that the data applies to. If not specified, then it is inherited from the dataset's publisher. Formatted as a GeoJSON point, bounding box or polygon. |
| theme | dcat:theme | `http://eurovoc.europa.eu/209416` - Police - Eurovoc <br> `COFOG/03.1.0` Police services - COFOG <br> `http://id.esd.org.uk/function/20` - Police services - ESD | Main thematic category of the dataset. Preferably expressed as a URI from a known vocabulary: <br> [Eurovoc](http://eurovoc.europa.eu/) <br> [COFOG](https://github.com/datasets/cofog/blob/master/data/cofog.csv) <br> [ESD Service](http://standards.esd.org.uk/?uri=list%2Fservices)/[Function](http://standards.esd.org.uk/?uri=list%2Ffunctions) <br>More than one can be specified using a `[]` list. |

Local Authorities should ensure they add an ESD service or function URI as the dataset's theme, to ensure good classification.

## Distribution

These are for **data** not documentation.

| data.json field | DCAT predicate | Example value | Comments |
| --------------- | -------------- | ------------- | -------- |
| downloadURL | dcat:downloadURL | `http://site.gov.uk/river-levels/dec2012.csv` | The direct URL that downloads a file with the data |
| accessURL | dcat:accessURL | `http://www.site.gov.uk/api/sparql` <br> `http://site.gov.uk/river-level-data.html` | If there is not a downloadURL, specify the accessURL, which is the URL of an API or other non-downloadable data (NB documentation or other web pages should not be distributions) |
| title | dct:title | Spend transactions, Dec 2012 |
| description | dct:description | | Not currently displayed on DGU |
| format | dcat:mediaType | `text/csv` | Currently recognized mime-types are listed here: [DGU formats](https://github.com/datagovuk/ckanext-dgu/blob/master/ckanext/dgu/lib/formats.py#L86) |
| conformsTo | dct:conformsTo | `http://schemas.opendata.esd.org.uk/publictoilets/PublicToilets.json?v=0.41` | URL of the machine-readable schema that the data conforms to. See: [Harvesting Guide - Appendix E - Local Authority data schemas (PDF)](http://data.gov.uk/sites/default/files/library/Harvesting%20guide.pdf). |
| temporal | dct:temporal | 2000-01-15/2010-01-15 | The date period that the data applies to. Formatted as two ISO 8601 dates (or datetimes) separated by a slash. |
| spatial |  dct:spatial | `{\"type\":\"Polygon\",\"coordinates\":[[[2.072, 49.943],[2.072, 55.816], [-6.236, 55.816], [-6.236, 49.943], [2.072, 49.943]]]}` | The geographic location that the data applies to. If not specified, then it is inherited from the dataset if not its publisher. Formatted as a GeoJSON point, bounding box or polygon. |

## data.json file structure

The data.json file should be structured as an array "[ ... ]" of dataset objects "{ ... }".

e.g.
```
[
 {"title": "Live traffic information from the Highways Agency",
  "license": "No license provided",
  "publisher": {"name": "Highways Agency", "mbox": "test@test.com"},
  "distribution": [
    {
      "downloadURL": "https://s3-eu-west-1.amazonaws.com/lmtesting2810/HATRIS_15MinuteMIDAS_YYYY-MM-DD_0.csv.zip",
      "title": "Hatris 15 Min Midas 0",
      "format": "application/zip"
    }]
 },
 {"title": "Roadworks locations",
   ...
 }
]

```

Alternatively, you can put this information inside a catalogue object under the key "datasets". Although data.gov.uk doesn't harvest the catalogue data, allowing this structure gives reasonable compatibility with Project Open Data.

## Character encoding

The data.json file should have ASCII or UTF-8 character encoding (as per the JSON standard). The harvester now detects this and gives an error if it is not acceptable.

## Namespaces

| dcat | http://www.w3.org/ns/dcat# |
| dct | http://purl.org/dc/terms/ |
| rdfs | http://www.w3.org/2000/01/rdf-schema# |

## Comparison with USA data.json schema

The UK data.json format is based on the Project Open Data data.json schema used by the U.S. Federal Government and agencies: <https://project-open-data.cio.gov/v1.1/schema/>

However there are a few conscious differences that are listed here for reference:

| Field | Change | Explanation |
| ----- | ------ | ----------- |
| bureauCode, programCode, primaryITInvestmentUII, systemOfRecords, dataQuality | not required | They are codes specific to US Federal Government |
| temporal, spatial | Can be applied to not just a dataset but also distributions. | e.g. Spend data is split up by months |
| spatial | Formatted with GeoJSON | GeoJSON is analagous to GML but preferred. Place name strings are not preferred as they can be ambiguous. |
| theme | The values are URIs from known classification vocabularies, rather than simply strings. | A catalogue chooses its own classification vocabularies, so there is little value in simple strings determined by the data publisher. Strings related to the data's topic can go in the keywords field. |

