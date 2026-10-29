# 2026 柴古唐斯 50K · Tsaigu Trail

以用户提供的 `2026柴古唐斯50K.gpx` 为来源，展示真实赛道的 3D 地形、卫星影像、四站地标和联动海拔剖面。

- 页面发布地址：[2026 柴古唐斯 50K](https://joker-outdoor.github.io/tsaigu-trail-50k-2026/)（合入并完成 Pages 发布后可访问）。
- 源码：[joker-outdoor/tsaigu-trail-50k-2026](https://github.com/joker-outdoor/tsaigu-trail-50k-2026)。
- [下载路线 GPX](data/route.gpx)：保留全部 7,984 个轨迹点的坐标和海拔，移除原文件中的账号、时间及扩展信息。

## 路线与数据口径

**50K 是赛事组别名称；页面距离为 GPX 坐标测算的 49.27 km**，未四舍五入值为 49.266483624709686 km。轨迹没有按 50 km 缩放，也没有配置按官方距离比例定位的公里牌。

原始 GPX 海拔范围为 **−13 至 1,121 m**，剖面保留这些原值；3D 轨迹贴合 DEM 地形。地形高度默认夸张 3 倍，可用控件调回 1 倍。负海拔未被截断或修正。

输入没有可靠的活动日期、逐点时间、用时或正式累计爬升 / 下降统计，相应数据保持缺失，不推算配速或用海拔噪声累加正式爬升。标题中的 2026 是路线版本标识；配置 `madeOn` 为页面制作日期，`recordedOn` 为空，不作为活动日期。未使用健康数据库。

四站按行进顺序取自原 GPX 的具名路标，以 WGS84 经纬度吸附到最近的原始赛道点；下表索引从 0 开始。名称及补给 / 补水类型沿用输入，不补造 CP 编号。地标里程使用吸附点的轨迹几何里程。

| 站点 | GPX 路标经度 | GPX 路标纬度 | 吸附轨迹点索引 |
| --- | --- | --- | --- |
| 法云寺补给站 | 121.0799166 | 28.8166551 | 2140 |
| 道场基补水站 | 121.0307217 | 28.8099994 | 3804 |
| 杨杜村补给站 | 121.0151549 | 28.8487675 | 5473 |
| 荣村补给站 | 121.0723429 | 28.8429195 | 6818 |

路线配置见 [route.config.json](route.config.json)，生成资产位于 [data/](data/)。全景卫星影像使用 Esri World Imagery z14，地形使用 AWS 托管的 Terrarium DEM z13；沿线近景使用 Esri z17，共 **74 块**，索引为 [imagery-detail.json](data/imagery-detail.json)。这些是实际下载的影像和地形，不是模板合成资产。浏览器读取仓库内的资产，无需地图 API key；重新生成时才需网络请求。

## 使用

在仓库根目录启动静态服务器：

```sh
python3 -m http.server 8080 --bind 127.0.0.1
```

浏览器访问 `http://localhost:8080`；`?view=2d` 强制俯视降级模式。直接以文件方式打开不支持 ES modules / fetch。

页面保留模板的全景、俯视、旋转、地形高度、卫星底图、遮挡段、路线着色、地标点击、GPX 下载、剖面联动、手机布局及 2D 降级功能。「沿线飞行」需主动点击，支持暂停、拖进度与 1 / 2 / 4 倍模拟速度；动画时长与倍速不代表真实比赛用时或运动速度。缺少时间数据时速度着色不可用。

近景按需加载，远景和 2D 使用全景底图；局部影像加载失败时回退到全景。此路线不含城市建筑数据。模板的 `video.html` 和导出工具保留，本项目未生成 MP4。

## 验证与重新生成

```sh
python3 -m pip install -r requirements.txt
python3 scripts/check_data.py
node --check app.js
node --check video.js
node --test scripts/test_route_style.mjs
python3 -m unittest discover -s scripts -p 'test_*.py'
```

如需从原始输入重新生成，原文件留在仓库外或已忽略的 `inputs/`，使用本路线配置，不加 `--demo`：

```sh
python3 scripts/build_data.py /path/to/2026柴古唐斯50K.gpx --config route.config.json --output data
```

原始输入未提交；公开 GPX 仅保留轨迹名、坐标和海拔。自动检查覆盖数据格式、影像索引、生成器及模板交互逻辑；真实 WebGL 画面、手机布局和影像接缝需另用浏览器验看。

场景测试默认使用 `examples/fixtures/` 中固定的合成路线与地形，取自本仓库初始基线 `f4b8dc7e642aa015f84348bdbda4ae0804bcb947`，避免替换真实赛道后改变测试前提；公开路线仍由数据校验与真实赛道 probe 验证。

发布使用 GitHub Pages，来源为 `main` 分支、仓库根目录 `/`；通过 PR 合入后由工作流启用并核验发布，以上发布链接不表示此文档已完成上线验收。

## 模板与署名

复用 [joker-outdoor/route-visualization-template](https://github.com/joker-outdoor/route-visualization-template/tree/35c05aecfedbebe35dcb76ac323a5ad03a576281)，模板版本固定为 **`35c05aecfedbebe35dcb76ac323a5ad03a576281`**。本路线仅替换配置、数据、静态标题与说明，保留完整渲染实现及功能。模板源自 [龙泉七湖连穿](https://github.com/joker-outdoor/longquan-seven-lakes-2026-10-02)，亦可参见 [贡嘎 100](https://github.com/joker-outdoor/gongga100-2026)。

- Three.js 0.160.0 官方原文件，MIT，见 [vendor/LICENSE](vendor/LICENSE)。
- 卫星底图：[Esri World Imagery](https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer)。Imagery © Esri, Maxar, Earthstar Geographics, and the GIS User Community。影像版权归各提供者，不随模板代码再许可。
- DEM：[Tilezen / Mapzen Terrain Tiles](https://github.com/tilezen/joerd)，Terrarium 瓦片由 AWS 托管；Terrain · Mapzen; SRTM / GMTED2010 courtesy of USGS。保留[对应数据署名与许可](https://github.com/tilezen/joerd/blob/master/docs/attribution.md)。
