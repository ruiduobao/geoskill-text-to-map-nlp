# Natural Language to Map (geoskill-text-to-map-nlp)

> Parses natural-language keywords, extracts parameters, and renders with matplotlib to output a thematic map PNG/GeoJSON (offline numpy-equivalent implementation)

---

## 1. Overview

Automatically generates a thematic map from a single natural-language sentence (e.g., "generate a night-light map of Shanghai"): parses the intent to select the layer type (vegetation/elevation/light/water/temperature/land cover) and color scheme, renders a map PNG with geographic extent, color bar and title, and also outputs the layer GeoTIFF and boundary GeoJSON. This skill is an **offline numpy-equivalent implementation** of an LLM/NL2Map system: it does not depend on large models or the network, reproducing the natural-language cartography pipeline with "keyword rule parsing (equivalent of intent recognition + slot filling) → physically consistent layer synthesis → matplotlib Agg rendering"; the parsing results (layer/color scheme/place name/title) are written to disk for auditability.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-text-to-map-nlp.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `map.png` | PNG | Rendered thematic map (color bar + title + lat/lon axes) |
| `layer.tif` | GeoTIFF | Map layer raster |
| `footprint.geojson` | GeoJSON | Map-extent boundary polygon |
| `parsed_query.json` | JSON | Parsed layer/color scheme/place name/title parameters |
| `output-manifest.json` | JSON | Run manifest (inputs/outputs/QA/exit code) |

## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
