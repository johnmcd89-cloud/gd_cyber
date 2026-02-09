# 65.4 Mapping and geolocation (high-level)

Location information is unusually persuasive.

A point on a map looks objective.

In reality, a location estimate is a measurement with uncertainty.

Different tools and data sources can produce different locations for the same device, even when everyone is acting honestly.

This section introduces mapping and geolocation as investigation tools.

It is intentionally high-level.

The goal is to help you:

choose appropriate tool categories,

understand common data sources and their limitations,

and log location evidence in a way that remains credible later.

All techniques described here assume legal authorization and ethical use.

---

## 65.4.1 Definitions (mapping, geolocation, coordinate system, accuracy)

**Mapping** is the practice of representing geographic information visually.

A map is a model.

It is useful because it compresses complexity.

It is dangerous when it hides uncertainty.

**Geolocation** is the process of estimating a device’s position.

Geolocation can be obtained from satellite navigation, local sensors, network identifiers, or media artifacts.

A **coordinate system** is a rule set for representing a location as numbers.

For example, latitude and longitude are coordinate components.

A **coordinate reference system** is a coordinate system plus a definition of how it relates to the Earth.

The GeoJSON standard explicitly uses the World Geodetic System (WGS) 1984 (often written “WGS 84”) coordinate reference system and expresses coordinates in decimal degrees. [S1]

GeoJSON is a geospatial data interchange format based on JavaScript Object Notation (JSON), which is a widely used text format for structured data. [S1]

A **datum** is a reference model used to interpret coordinates.

WGS 84 is a widely used global standard for cartography and satellite navigation, including the Global Positioning System. [S5]

**Accuracy** describes how close a measurement is to the true value.

**Precision** describes how consistent repeated measurements are.

A system can be precise but inaccurate.

For investigation work, you must treat accuracy as evidence, not as an assumption.

The World Wide Web Consortium (W3C) Geolocation specification explicitly treats geolocation as an interface that provides access to location information and includes fields such as accuracy in its coordinate model. [S6]

---

## 65.4.2 Tool categories

Mapping and geolocation are not one tool.

They are an ecosystem.

Different tools exist for different levels of fidelity.

### Geographic information system (GIS) tools

A **geographic information system (GIS)** tool is software for viewing, editing, analyzing, and exporting geospatial datasets.

GIS tools are useful when you need repeatability.

They let you store data layers, document transformations, and export results in standard formats.

QGIS is a widely used free and open-source GIS tool, positioned as a spatial visualization and decision-making platform. [S7]

### Web maps and imagery viewers

Web maps are fast.

They are useful for orientation and for comparing hypotheses against visible terrain.

They are also a dependency risk.

If the network disappears, the map disappears.

For field work, treat offline capability as a first-class requirement.

### OpenStreetMap and community map ecosystems

OpenStreetMap is a community-maintained map.

Its wiki reflects a living ecosystem of mapping practice and tooling. [S8]

Community maps can be particularly useful for humanitarian mapping, local detail, and transparency.

They can also vary in completeness by region.

### Media geolocation tools

Media artifacts can contain location clues.

A photo or video might contain:

visual landmarks,

shadows indicating sun position,

street signage,

or embedded metadata.

Bellingcat’s beginner geolocation guide describes a workflow for confirming the location a video was filmed by using visible landmarks and comparing them to satellite imagery from mapping services. [S9]

This category of work often overlaps with open-source intelligence.

### Radio and network-assisted location aids

Some geolocation comes from radios and networks.

Examples include:

satellite navigation receivers,

cellular network information,

Wi-Fi access point identifiers,

and Bluetooth beacon identifiers.

These methods can be powerful.

They can also create privacy risk.

They should only be used when authorized and when the investigation scope justifies them.

---

## 65.4.3 Data sources

A location estimate is only as credible as its data sources.

In practice, geolocation data sources fall into a few repeatable categories.

### Global navigation satellite systems

A **global navigation satellite system (GNSS)** is a satellite-based system that provides positioning coverage.

Satellite navigation literature describes GNSS systems as providing global coverage and lists major systems such as the United States Global Positioning System (GPS), Russia’s Global Navigation Satellite System (GLONASS), China’s BeiDou, and the European Union’s Galileo. [S10]

GNSS is often the most familiar location source.

However, it can be degraded by:

poor sky visibility,

radio interference,

or device antenna limitations.

### Device and application logs

Devices often log location-related events.

Examples include:

a navigation application storing a route history,

an operating system recording “last known location,”

or a radio device logging a fix.

Logs can be incomplete.

They can also be altered.

Treat logs as evidence that must be verified and cross-checked.

### Media metadata (Exif)

Many image files include metadata.

Exchangeable image file format (Exif) is a standard that specifies formats and metadata tags used by digital cameras and related systems. [S11]

Exif metadata can include timestamps, device models, and sometimes location.

It can also be absent, stripped, or misleading.

Use it as a clue, not as proof.

### Network identifiers

Some geolocation systems use network identifiers.

For example, Wi-Fi access points can be identified by a hardware address.

External databases can sometimes map those identifiers to approximate locations.

The obvious limitation is that databases can be wrong, outdated, or incomplete.

The ethical limitation is that this approach can turn incidental network observations into location tracking.

If you use this method, document authorization and scope.

### Public map and imagery sources

Public basemaps and imagery are data sources too.

They have versions.

They have update cycles.

They have attribution and licensing rules.

If you base a conclusion on an image tile, record where it came from and when you accessed it.

---

## 65.4.4 Evidence logging for location claims

Location evidence is easy to lose.

It is also easy to misinterpret.

The goal of evidence logging is to make your location claim reconstructable.

### Record coordinates and reference system explicitly

Always record:

the latitude and longitude,

the coordinate reference system,

and the units.

GeoJSON is a practical interchange format because it is standardized and explicitly tied to WGS 84 with decimal degrees. [S1]

### Record uncertainty

If a tool reports accuracy, record it.

If a tool does not report accuracy, record that absence.

It is better to admit uncertainty than to imply false precision.

### Record time and time source assumptions

Location is usually time-dependent.

A moving device can travel far in minutes.

Record timestamps in a standard format.

Request for Comments (RFC) 3339 defines a date and time format for use in Internet protocols as a profile of ISO 8601. [S12]

ISO is short for the International Organization for Standardization.

Normalize to Coordinated Universal Time (UTC) when possible.

Record your assumptions about clock synchronization.

### Record provenance

A location claim should include provenance notes.

At minimum:

who collected the evidence,

how it was collected,

what raw sources were used,

and what transformations were applied.

The National Institute of Standards and Technology (NIST) forensic techniques guide (NIST Special Publication (SP) 800-86) highlights that effective forensics involves multiple data sources (files, operating systems, network traffic, applications) and cautions that recommended practices should be applied only with appropriate legal and management consultation. [S13]

This is a reminder that provenance is not only technical.

It is also organizational.

### Export defensible artifacts

Prefer exporting artifacts that can be re-opened.

A screenshot can be helpful, but it is not a dataset.

If possible, export:

GeoJSON features,

comma-separated value (CSV) tables of points,

and a short narrative log describing the claim.

If you later need to share evidence, these formats preserve meaning.

---

## 65.4.5 Community signals and use cases

Community geolocation work often demonstrates a practical truth.

Many location claims are supported by small, converging clues.

In open-source intelligence (OSINT) communities, practitioners share workflows for combining public satellite imagery with external data sources such as sensor feeds and public records to infer locations.

Community examples emphasize reproducibility by describing steps so others can validate the claim. [S14]

This is a cultural point with operational value.

If you want your claim to be trusted, you must make it checkable.

---

## 65.4.6 Suggested figures

1) **Tool taxonomy chart**: GIS tools, web map viewers, offline maps, media geolocation, and radio/network-assisted methods.

2) **Coordinate reference diagram**: latitude/longitude on WGS 84, with an explanation of “datum” and “decimal degrees.”

3) **Evidence card template**: fields for coordinates, accuracy, timestamp, time zone, source, and provenance notes.

4) **Triangulation sketch**: a location claim supported by GNSS data, imagery comparison, and a log artifact, showing where uncertainty is recorded.

---

## Sources

- [S1] RFC 7946: *The GeoJSON Format* (GeoJSON definition; uses WGS 84 and decimal degrees) — https://www.rfc-editor.org/rfc/rfc7946
- [S2] W3C: *Geolocation* (device geolocation interface; coordinate/accuracy fields) — https://www.w3.org/TR/geolocation/
- [S3] Wikipedia: *World Geodetic System* (WGS 84 as a global mapping/navigation standard) — https://en.wikipedia.org/wiki/World_Geodetic_System
- [S4] Wikipedia: *Exchangeable image file format (Exif)* (metadata tags for images and related media) — https://en.wikipedia.org/wiki/Exchangeable_image_file_format
- [S5] Wikipedia: *Satellite navigation* (GNSS definition and major systems) — https://en.wikipedia.org/wiki/Satellite_navigation
- [S6] QGIS: *Spatial without Compromise* (GIS tool category overview and capabilities framing) — https://www.qgis.org/
- [S7] OpenStreetMap Wiki: *Main Page* (community mapping ecosystem and tooling entry point) — https://wiki.openstreetmap.org/wiki/Main_Page
- [S8] Bellingcat: *A Beginner’s Guide to Geolocating Videos* (community-oriented geolocation workflow using landmarks and imagery) — https://www.bellingcat.com/resources/how-tos/2014/07/09/a-beginners-guide-to-geolocation/
- [S9] RFC 3339: *Date and Time on the Internet: Timestamps* (standard timestamp format; ISO 8601 profile) — https://www.rfc-editor.org/rfc/rfc3339
- [S10] NIST SP 800-86: *Guide to Integrating Forensic Techniques into Incident Response* (multiple data sources; legal/management consultation caution) — https://csrc.nist.gov/pubs/sp/800/86/final
- [S11] r/OSINT: search results for “geolocation” (community workflows and reproducible geolocation write-ups) — https://old.reddit.com/r/osint/search?q=geolocation&restrict_sr=on&sort=relevance&t=all
