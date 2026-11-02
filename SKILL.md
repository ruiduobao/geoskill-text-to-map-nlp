---
name: geoskill-text-to-map-nlp
description: '自然语言关键词解析+参数提取+matplotlib渲染，输出专题地图PNG/GeoJSON（离线numpy等价实现）'
---

# 自然语言生成地图 | Natural Language to Map

Automatically generates a thematic map from a single natural-language sentence (e.g., "generate a nighttime-light map of Shanghai"): parse the intent to select the layer type (vegetation / elevation / nighttime lights / water / temperature / land features) and color scheme, render a map PNG with geographic extent, color bar, and title, and output the layer GeoTIFF and footprint GeoJSON.

This skill is an **offline numpy-equivalent implementation** of an LLM/NL2Map system: with no large-model or network dependency, it reproduces the natural-language mapping pipeline via keyword-rule parsing (the equivalent of intent recognition + slot filling) → physically consistent layer synthesis → matplotlib Agg rendering; the parsed results (layer / color scheme / place name / title) are written to disk for auditability.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy scikit-learn geopandas shapely matplotlib
```

## Usage / 使用方法

### Example 1 (synthetic data, offline)

```bash
python geoskill-text-to-map-nlp.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2: Synthetic-layer mapping (offline)

```bash
python geoskill-text-to-map-nlp.py --bbox 116.0 39.0 117.0 40.0 --synthetic --query "Beijing vegetation index" --output-dir ./out
```

### Example 3: Layer selection via natural language

```bash
python geoskill-text-to-map-nlp.py --bbox 121.0 31.0 122.0 32.0 --query "Shanghai nighttime light distribution" --output-dir ./out
```

### Example 4: Force-specify the layer type

```bash
python geoskill-text-to-map-nlp.py --bbox 116.0 39.0 117.0 40.0 --layer elevation --query "terrain map" --output-dir ./out
```

### Example 5: Direct rendering of a real raster

```bash
python geoskill-text-to-map-nlp.py --input ndvi.tif --query "NDVI of the study area" --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `map.png` | PNG | Rendered thematic map (color bar + title + lat/lon axes) |
| `layer.tif` | GeoTIFF | Map layer raster |
| `footprint.geojson` | GeoJSON | Map extent boundary polygon |
| `parsed_query.json` | JSON | Parsed layer / color scheme / place name / title parameters |
| `output-manifest.json` | JSON | Run manifest (inputs/outputs/QA/exit code) |

## Data Source / 数据源 / Source

A local GeoTIFF (used directly as the layer), or a simulated layer of the corresponding type synthesized from the parsed `--query` result.

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is performed locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
