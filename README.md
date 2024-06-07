# Raudikko Analysis for OpenSearch

The Raudikko Analysis plugin provides Finnish language analysis using [Raudikko](https://github.com/EvidentSolutions/raudikko).

## Supported versions

| Plugin version | Raudikko version | OpenSearch version                                                        |
|----------------|------------------|---------------------------------------------------------------------------|
| 0.1.0          | 0.1.4            | 2.5.0, 2.6.0, 2.7.0, 2.8.0, 2.9.0, 2.10.0, 2.11.0, 2.12.0, 2.13.0, 2.14.0 |

The plugin should support all 2.x.x, but OpenSearch requires the plugin to declare an exact version, so each release
of the plugin has several packages per OpenSearch version. Check [releases](https://github.com/EvidentSolutions/opensearch-analysis-raudikko/releases)
if the version you need is included, and if not, please create an issue requesting support for that version.

## Installing

To install the plugin, run the following command, changing the version in URL to match your version of OpenSearch:

```console
bin/opensearch-plugin install https://github.com/EvidentSolutions/opensearch-analysis-raudikko/releases/download/v0.1.0/opensearch-analysis-raudikko-0.1.0-os2.9.0.zip
```

### Docker

Building the plugin and running with docker

```console
./gradlew build -DopensearchVersion=2.14.0
cd etc
docker compose build --build-arg "OS_VERSION=2.14.0"
docker compose up
```

OpenSearch should be running and available at port 9200.

### Verify installation

After installing the plugin, you can quickly verify that it works by executing:

```console
curl -XGET 'localhost:9200/_analyze' -H 'Content-Type: application/json' -d '
{
  "tokenizer" : "finnish",
  "filter" : [{"type": "raudikko"}],
  "text" : "Testataan raudikon analyysiä tällä tavalla yksinkertaisesti."
}'
```

If this works without error messages, you can proceed to configure the plugin index.

## Configuring

Include `finnish` tokenizer and `raudikko` filter in your analyzer, for example:

```json
{
  "index": {
    "analysis": {
      "analyzer": {
        "default": {
          "tokenizer": "finnish",
          "filter": ["lowercase", "raudikkoFilter"]
        }
      },
      "filter": {
        "raudikkoFilter": {
          "type": "raudikko"
        }
      }
    }
  }
}
```

You can use the following filter options to customize the behaviour of the filter:

| Parameter           | Default value | Description                                      |
|---------------------|---------------|--------------------------------------------------|
| analyzeAll          | true          | Use all analysis possibilities or just the first |
| splitCompoundWords  | false         | Split analysed compound words to its parts       |
| minimumWordSize     | 3             | minimum length of words to analyze               |
| maximumWordSize     | 100           | maximum length of words to analyze               |
| analysisCacheSize   | 1024          | number of analysis results to cache              |

## License and copyright

Copyright (C) 2021-2023  Evident Solutions Oy

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
