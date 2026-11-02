# 自然语言生成地图 | Natural Language to Map

用一句自然语言（如"生成上海的夜间灯光地图"）自动生成专题地图：解析意图选择图层类型（植被/高程/灯光/水体/温度/地物）与配色，渲染带地理范围、色条与标题的地图 PNG，同时输出图层 GeoTIFF 与边界 GeoJSON。

本 skill 是 LLM/NL2Map 系统的**离线 numpy 等价实现**：不依赖大模型与网络，用"关键词规则解析（意图识别 + 槽位填充的等价物）-> 物理一致的图层合成 -> matplotlib Agg 渲染"复现自然语言制图流程；解析结果（图层/配色/地名/标题）落盘可审计。

## 依赖

```bash
pip install numpy rasterio scipy scikit-learn geopandas shapely matplotlib
```

## 使用方法

### 示例 1（合成数据，离线）

```bash
python geoskill-text-to-map-nlp.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### 示例 2：合成图层制图（离线）

```bash
python geoskill-text-to-map-nlp.py --bbox 116.0 39.0 117.0 40.0 --synthetic --query "北京植被指数" --output-dir ./out
```

### 示例 3：自然语言选图层

```bash
python geoskill-text-to-map-nlp.py --bbox 121.0 31.0 122.0 32.0 --query "上海夜间灯光分布" --output-dir ./out
```

### 示例 4：强制指定图层类型

```bash
python geoskill-text-to-map-nlp.py --bbox 116.0 39.0 117.0 40.0 --layer elevation --query "地形图" --output-dir ./out
```

### 示例 5：真实栅格直接渲染

```bash
python geoskill-text-to-map-nlp.py --input ndvi.tif --query "研究区 NDVI" --output-dir ./out
```

## 输出

| 文件 | 格式 | 说明 |
|---|---|---|
| `map.png` | PNG | 渲染好的专题地图（色条 + 标题 + 经纬度轴） |
| `layer.tif` | GeoTIFF | 地图图层栅格 |
| `footprint.geojson` | GeoJSON | 地图范围边界多边形 |
| `parsed_query.json` | JSON | 解析出的图层/配色/地名/标题参数 |
| `output-manifest.json` | JSON | 运行清单（输入/输出/QA/退出码） |

## 数据源 / Source

本地 GeoTIFF（直接作为图层），或按 --query 解析结果合成对应类型的模拟图层。

## 隐私声明 / Privacy

- 默认离线运行，`--synthetic` 模式完全无网络。
- 所有处理在本地完成，不上传任何用户数据。

## License

MIT
