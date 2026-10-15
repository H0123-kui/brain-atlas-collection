# Brain Atlas Collection

人脑功能性图谱归档，收录物体识别、工作记忆、梭状回面孔选择性区域和视觉运动相关的四套研究资源。各数据包保留原始文件名、压缩格式和包内说明，适用于功能定位、ROI 定义、空间先验构建及跨研究的脑区对应分析。

本 README 的实验范式与图谱定义依据包内原始说明，文件数量、数组维度、空间矩阵和标签编码依据实际数据读取；研究背景与正式引用另以原始论文为依据。下文的包内路径均指解压后的相对路径。

## 目录

- [图谱总览](#图谱总览)
- [下载与解压](#下载与解压)
- [图像类型与命名约定](#图像类型与命名约定)
- [BAA-OR：物体识别功能图谱](#baa-or物体识别功能图谱)
- [BAA-WM：工作记忆功能图谱](#baa-wm工作记忆功能图谱)
- [HCP-FFA：梭状回面孔选择性图谱](#hcp-ffa梭状回面孔选择性图谱)
- [MT_atlas：人类视觉运动复合区图谱](#mt_atlas人类视觉运动复合区图谱)
- [空间与数据读取](#空间与数据读取)
- [分析与报告约定](#分析与报告约定)
- [原始资料与参考文献](#原始资料与参考文献)

## 图谱总览

| 数据包 | 主要研究对象 | 数据表示 | 典型用途 |
| --- | --- | --- | --- |
| [BAA-OR.zip](BAA-OR.zip) | 面孔、场景和物体选择性区域 | MNI152 体积 NIfTI；fsaverage 表面 NIfTI | 类别选择性定位、概率 ROI、群体响应强度与个体变异分析 |
| [BAA-WM.zip](BAA-WM.zip) | `2-back > 1-back` 工作记忆激活网络 | MNI 标准空间体积 NIfTI | 双侧额叶、顶叶、岛叶和扣带回 ROI 定义 |
| [HCP-FFA.7z](HCP-FFA.7z) | `pFus-faces/FFA-1` 与 `mFus-faces/FFA-2` | `32k_fs_LR` 与 fsaverage 的 CIFTI | 个体 FFA、概率图、最大概率分区、FFC 二分和拓扑分组分析 |
| [MT_atlas.rar](MT_atlas.rar) | 人类视觉运动复合区 `hMT+` | fsaverage 表面 MGZ；MNI152 体积 NIfTI | 运动敏感区域的概率定位与阈值方案比较 |

解压后的科研影像共 **89 份**：37 份体积 NIfTI、38 份 fsaverage 顶点 NIfTI/MGZ、14 份 CIFTI。各包另保留原始说明；HCP-FFA 同时提供参与者标识与半球分组表。

### 归档文件大小

| 文件 | 大小 / bytes | 大小 / MiB |
| --- | ---: | ---: |
| `BAA-OR.zip` | 10,649,374 | 10.16 |
| `BAA-WM.zip` | 1,115,697 | 1.06 |
| `HCP-FFA.7z` | 1,091,302 | 1.04 |
| `MT_atlas.rar` | 199,431 | 0.19 |
| **合计** | **13,055,804** | **12.45** |

`1 MiB = 1,048,576 bytes`。表中大小为压缩包的实际文件长度。

## 下载与解压

使用具备仓库访问权限的 GitHub 账号克隆：

```bash
git clone https://github.com/H0123-kui/brain-atlas-collection.git
cd brain-atlas-collection
```

也可以通过上方文件链接下载所需数据包。使用提供 `7z` 命令的解压工具时，可将四套资源整理到统一目录：

```bash
7z x BAA-OR.zip -oextracted
7z x BAA-WM.zip -oextracted
7z x HCP-FFA.7z -oextracted
7z x MT_atlas.rar -oextracted
```

主要科研目录如下；具体文件类别在各图谱章节中展开：

```text
extracted/
|-- BAA-OR/
|   |-- BAA-OR-Design Paradigm and Data Acquisition.pdf
|   |-- face/{volume,surface}/
|   |-- object/{volume,surface}/
|   `-- scene/{volume,surface}/
|-- BAA-WM/
|   |-- BAA-WM Design Paradigm and Data Acquisition.pdf
|   `-- volume/
|-- HCP-FFA/
|   |-- readme.pdf
|   |-- subject_id.txt
|   |-- group_id.csv
|   `-- HCP-YA_FFA-*.nii
`-- MT_atlas/
    |-- ReadMe.txt
    |-- surface/
    `-- volume/
```

文件统计以科研影像和配套说明为口径。`BAA-WM.zip` 中的 `__MACOSX/`、`.DS_Store`、`._*` 属于 macOS 打包元数据，分析时按科研目录选择输入文件即可。

## 图像类型与命名约定

### BAA 图像类别

| 缩写 | 原始名称 | 含义与数值解释 |
| --- | --- | --- |
| `PAM` | Probabilistic Activation Map | 对个体任务激活图以 `Z > 2.3`、uncorrected 阈值二值化，再跨参与者平均，表示该位置出现显著任务激活的比例 |
| `PRM` | Probabilistic ROI Map | 对个体层面勾画的 ROI 计算空间出现频率，表示体素或顶点归属该 ROI 的概率 |
| `MPRM` | Maximum-probability ROI Map | 比较多个 ROI 的概率，将每个位置分配给概率最大的 ROI；正整数表示区域标签，`0` 表示背景 |
| `RMM` | Response Magnitude Map | 任务对比的 percent signal change，简称 PSC，跨参与者的均值 |
| `RVM` | Response Variability Map | 对应 PSC 跨参与者的标准差，表征响应的个体间变异 |

`PAM` 关注任务激活的出现比例，`PRM` 关注某个已定义 ROI 的空间出现比例。两者采用各自的构建流程，适合分别作为任务响应先验和特定脑区先验。

BAA 体积概率图采用 `0-1` 标度，例如 `0.25` 表示 25%。`RMM` 和 `RVM` 按原始 PSC 数值读取，保留图像中的实际标度。多通道 `PRM` 的每个通道独立表示一个 ROI，其空间范围可以重叠。

### 文件名标记

| 标记 | 含义 |
| --- | --- |
| `l` / `r`，`lh` / `rh` | 左 / 右半球 |
| `FvO` | Faces versus objects，面孔相对于物体 |
| `SvO` | Scenes versus objects，场景相对于物体 |
| `OvS` | Objects versus scrambled objects，完整物体相对于打乱物体 |
| `2backvs1back` | 2-back 相对于 1-back |
| `2mm` | 发布的体积图像采用 2 mm 各向同性采样 |
| `fsaverage` / `fsavg` | FreeSurfer fsaverage 表面空间 |
| `32k_fs_LR` | HCP fs_LR 的 32k 级表面网格 |
| `.nii.gz` | gzip 压缩的 NIfTI；具体数据含义由所在目录和图像头确定 |
| `.dlabel.nii` | CIFTI 离散标签图，内嵌标签名称与颜色表 |
| `.dscalar.nii` | CIFTI 连续标量图，本包用于 FFA 概率图 |
| `.mgz` | FreeSurfer 压缩 MGH，本包用于表面顶点数据 |

BAA 的 `MPRM-thr10`、`thr20`、`thr30` 分别对应最大概率门槛 `0.10`、`0.20`、`0.30`。HCP-FFA 的 `MPM_thr-*` 以百分数标记概率门槛。MT 文件中的 `thr3` 至 `thr6` 对应原始个体定位的统计阈值，详见 MT 章节。

## BAA-OR：物体识别功能图谱

### 研究内容与实验范式

原始说明：`BAA-OR/BAA-OR-Design Paradigm and Data Acquisition.pdf`，共 8 页。该资源通过动态视觉定位任务描述面孔、场景和物体加工相关的脑活动。面孔图谱研究与场景图谱研究均基于 202 名健康成人，分别见 [Zhen et al., 2015](https://doi.org/10.1016/j.neuroimage.2015.03.010) 和 [Zhen et al., 2017](https://pmc.ncbi.nlm.nih.gov/articles/PMC6866930/)。

| 项目 | 包内说明记载 |
| --- | --- |
| 刺激类别 | Faces、scenes、objects、scrambled objects |
| 扫描安排 | 3 个 block-design run，每个 run 198 s |
| 刺激区块 | 每类 18 s，每个区块包含 6 段 3 s 视频 |
| 静息区块 | 每个 run 的开始、中间和结尾各 18 s |
| 被试任务 | 被动观看视频 |
| 扫描仪 | Siemens Trio 3T，12 通道头线圈，北京师范大学影像中心 |
| 功能像 | GRE-EPI；TR 2,000 ms；TE 30 ms；翻转角 90 度 |
| 功能像采样 | 面内 3.1 x 3.1 mm，30 层，层厚 4.8 mm |
| 结构像 | MP-RAGE；TR/TE/TI = 2,530/3.39/1,100 ms；翻转角 7 度 |

原始处理流程使用 FSL FEAT，包含头动校正、脑提取、6 mm FWHM 空间平滑、强度归一化和 120 s 高通滤波；以 GLM 建模刺激条件及头动参数。各 run 的结果通过固定效应模型合并，结构配准与标准化使用 FLIRT/FNIRT，发布的体积图像采样为 MNI152 2 mm 网格。

### 实际文件组成

| 子目录 | 对比 | 体积影像 | 表面影像 | 合计 |
| --- | --- | ---: | ---: | ---: |
| `face/` | `FvO` | 5 | 14 | 19 |
| `object/` | `OvS` | 5 | 6 | 11 |
| `scene/` | `SvO` | 5 | 8 | 13 |
| **总计** | | **15** | **28** | **43** |

每个类别的 `volume/` 均包含 `PAM`、`PRM`、`MPRM-thr10`、`RMM` 和 `RVM` 五种图像。例如：

```text
BAA-OR/face/volume/
|-- BAA-OR-FvO-PAM-2mm.nii.gz
|-- BAA-OR-FvO-PRM-2mm.nii.gz
|-- BAA-OR-FvO-MPRM-thr10-2mm.nii.gz
|-- BAA-OR-FvO-RMM-2mm.nii.gz
`-- BAA-OR-FvO-RVM-2mm.nii.gz
```

各类别的 `surface/` 包含逐 ROI、逐半球的 `PRM`，以及左、右半球各一份阈值为 10% 的 `MPRM`。表面图谱由体积空间定义的个体 ROI 投影到 fsaverage 后汇总得到；其科学定义保留体积 ROI 的构建依据。

### 4D PRM 通道顺序

体积 `PRM` 的第 4 维是 **ROI 通道轴**。以下顺序直接来自原始 PDF。

**面孔：**`BAA-OR-FvO-PRM-2mm.nii.gz`，shape 为 `(91, 109, 91, 12)`。

| 通道序号 | Python 索引 | ROI | 半球 | 区域名称 |
| ---: | ---: | --- | --- | --- |
| 1 | 0 | `rOFA` | 右 | Occipital face area，枕叶面孔区 |
| 2 | 1 | `lOFA` | 左 | Occipital face area，枕叶面孔区 |
| 3 | 2 | `rpFus` | 右 | Posterior fusiform face region，后部梭状回面孔区 |
| 4 | 3 | `lpFus` | 左 | Posterior fusiform face region，后部梭状回面孔区 |
| 5 | 4 | `raFus` | 右 | Anterior fusiform face region，前部梭状回面孔区 |
| 6 | 5 | `laFus` | 左 | Anterior fusiform face region，前部梭状回面孔区 |
| 7 | 6 | `rpcSTS` | 右 | Posterior continuation of superior temporal sulcus，上颞沟后续部 |
| 8 | 7 | `lpcSTS` | 左 | Posterior continuation of superior temporal sulcus，上颞沟后续部 |
| 9 | 8 | `rpSTS` | 右 | Posterior superior temporal sulcus，上颞沟后部 |
| 10 | 9 | `lpSTS` | 左 | Posterior superior temporal sulcus，上颞沟后部 |
| 11 | 10 | `raSTS` | 右 | Anterior superior temporal sulcus，上颞沟前部 |
| 12 | 11 | `laSTS` | 左 | Anterior superior temporal sulcus，上颞沟前部 |

**场景：**`BAA-OR-SvO-PRM-2mm.nii.gz`，shape 为 `(91, 109, 91, 6)`。

| 通道序号 | Python 索引 | ROI | 区域名称 |
| ---: | ---: | --- | --- |
| 1 | 0 | `rTOS` | 右侧 transverse occipital sulcus，横枕沟相关场景区 |
| 2 | 1 | `lTOS` | 左侧 transverse occipital sulcus，横枕沟相关场景区 |
| 3 | 2 | `rRSC` | 右侧 retrosplenial cortex/complex，压后皮层相关区域 |
| 4 | 3 | `lRSC` | 左侧 retrosplenial cortex/complex，压后皮层相关区域 |
| 5 | 4 | `rPPA` | 右侧 parahippocampal place area，海马旁场景区 |
| 6 | 5 | `lPPA` | 左侧 parahippocampal place area，海马旁场景区 |

**物体：**`BAA-OR-OvS-PRM-2mm.nii.gz`，shape 为 `(91, 109, 91, 4)`。

| 通道序号 | Python 索引 | ROI | 区域名称 |
| ---: | ---: | --- | --- |
| 1 | 0 | `rLO` | 右侧 lateral occipital region，外侧枕叶物体选择性区域 |
| 2 | 1 | `lLO` | 左侧 lateral occipital region，外侧枕叶物体选择性区域 |
| 3 | 2 | `rpFs` | 右侧 posterior fusiform region，后部梭状回物体选择性区域 |
| 4 | 3 | `lpFs` | 左侧 posterior fusiform region，后部梭状回物体选择性区域 |

对应的体积 `MPRM` 实测非零标签集合分别为 `1-12`、`1-6`、`1-4`，背景值为 `0`。使用连续概率时读取对应 `PRM` 通道；使用互斥区域分配时读取 `MPRM`。

表面 `MPRM` 延续双半球的区域编码：面孔左半球为 `2, 4, 6, 8, 10, 12`，右半球为 `1, 3, 5, 7, 9, 11`；场景左半球为 `2, 4, 6`，右半球为 `1, 3, 5`；物体左半球为 `2, 4`，右半球为 `1, 3`。各表面图均以 `0` 为背景。

## BAA-WM：工作记忆功能图谱

### 研究内容与实验范式

原始说明：`BAA-WM/BAA-WM Design Paradigm and Data Acquisition.pdf`，共 5 页。图谱通过 `2-back > 1-back` 对比定义工作记忆相关激活。对应研究纳入 477 名健康成人，见 [Chen et al., 2021](https://doi.org/10.1016/j.neuroimage.2021.118301)。

| 项目 | 包内说明记载 |
| --- | --- |
| 定位任务 | n-back；匹配前 2 个或前 1 个试次的刺激时按键 |
| 刺激类别 | 面孔、场景、物体 |
| 扫描安排 | 2 个 block-design run；原始说明记载每个 run 168 s |
| 类别区块 | 每个 20 s，呈现同一类别的 10 张灰度图，每张 2 s |
| 核心对比 | `2-back > 1-back` |
| 个体 ROI 定位 | `Z > 2.3`、uncorrected；watershed 分区后，结合功能与解剖参照由评定者勾画 |
| 功能像采集 | Siemens Trio 3T；TR 2,000 ms；TE 30 ms；3.1 x 3.1 x 4.8 mm |
| 发布空间 | MNI 标准空间，2 x 2 x 2 mm |

处理流程包括头动校正、6 mm FWHM 平滑、强度归一化、120 s 高通滤波、GLM 及跨 run 固定效应汇总。个体 ROI 的绘制采用多位评定者协同核定流程。

### 脑区和文件

每个半球包含以下七类 ROI，文件名中的 `l`、`r` 指示半球。

| 缩写 | 英文名称 | 中文名称 |
| --- | --- | --- |
| `FP` | Frontal pole | 额极 |
| `MFG` | Middle frontal gyrus | 额中回 |
| `FEF` | Frontal eye field | 额眼区 |
| `SPL` | Superior parietal lobule | 顶上小叶 |
| `INS` | Insular cortex | 岛叶皮层 |
| `PCUN` | Precuneus | 楔前叶 |
| `ACC` | Anterior cingulate cortex | 前扣带皮层 |

`BAA-WM/volume/` 共 17 个三维 NIfTI：

| 图像类别 | 数量 | 文件名规则 |
| --- | ---: | --- |
| 单个 ROI 的 `PRM` | 14 | `BAA-WM-2backvs1back-{l,r}{ROI}-PRM-2mm.nii.gz` |
| 全任务 `PAM` | 1 | `BAA-WM-2backvs1back-PAM-2mm.nii.gz` |
| 20% 门槛 `MPRM` | 1 | `BAA-WM-2backvs1back-MPRM-thr20-2mm.nii.gz` |
| 30% 门槛 `MPRM` | 1 | `BAA-WM-2backvs1back-MPRM-thr30-2mm.nii.gz` |

所有图像 shape 为 `(91, 109, 91)`。两个 `MPRM` 均包含背景 `0` 和非零标签 `1-14`。逐脑区分析可直接选用带明确半球与区域缩写的独立 `PRM`，例如 `BAA-WM-2backvs1back-lMFG-PRM-2mm.nii.gz`。

### 本包 MPRM 标签编码

下表来自本包 `MPRM` 与 14 个独立 `PRM` 的逐体素数值对应：对每个非零标签内的体素，识别概率最大的独立 PRM；20% 和 30% 两个版本中，各标签均得到一致的 ROI 对应。体素数按原始 2 mm 网格统计。

| 标签 | ROI | 半球与脑区 | thr20 体素数 | thr30 体素数 |
| ---: | --- | --- | ---: | ---: |
| 1 | `rSPL` | 右侧顶上小叶 | 2,747 | 1,602 |
| 2 | `lSPL` | 左侧顶上小叶 | 2,312 | 1,349 |
| 3 | `rPCUN` | 右侧楔前叶 | 1,058 | 553 |
| 4 | `lPCUN` | 左侧楔前叶 | 805 | 407 |
| 5 | `rFEF` | 右侧额眼区 | 2,071 | 1,286 |
| 6 | `lFEF` | 左侧额眼区 | 1,785 | 1,093 |
| 7 | `rMFG` | 右侧额中回 | 2,380 | 1,130 |
| 8 | `lMFG` | 左侧额中回 | 2,216 | 1,331 |
| 9 | `rACC` | 右侧前扣带皮层 | 1,458 | 960 |
| 10 | `lACC` | 左侧前扣带皮层 | 912 | 535 |
| 11 | `rINS` | 右侧岛叶皮层 | 758 | 497 |
| 12 | `lINS` | 左侧岛叶皮层 | 694 | 429 |
| 13 | `rFP` | 右侧额极 | 1,236 | 604 |
| 14 | `lFP` | 左侧额极 | 1,149 | 527 |

这两份标签图的存储 dtype 为 `float32`，实际类别值为整数；按离散标签图处理其数值语义。

## HCP-FFA：梭状回面孔选择性图谱

### 研究内容与定位依据

原始说明：`HCP-FFA/readme.pdf`，共 10 页。该资源来自 HCP Young Adult、S1200 数据发布，包含 1,053 位参与者、2,106 个半球的 FFA 定义与群体汇总。正式论文见 [Chen et al., 2023](https://doi.org/10.1016/j.neuroimage.2022.119765)。

| 区域 | 原始命名 | 解剖定位依据 |
| --- | --- | --- |
| 后部梭状回面孔区 | `pFus-faces/FFA-1` | 梭状回后部，延伸至 occipito-temporal sulcus，简称 OTS |
| 中部梭状回面孔区 | `mFus-faces/FFA-2` | 与 mid-fusiform sulcus，简称 MFS，的前端相关 |

原始流程综合个体皮层沟回与 `faces versus others` 激活，采用 `Z > 1.65`、`p < 0.05`、uncorrected 作为手工定位的功能参照。论文与配套说明报告，95.44% 的半球可识别两个梭状回面孔选择性区域。

| 项目 | 包内说明记载 |
| --- | --- |
| 扫描平台 | HCP 定制 Siemens Skyra 3T，32 通道线圈 |
| 结构像 | T1w/T2w，0.7 mm 各向同性 |
| 功能像 | GRE-EPI；TR 720 ms；TE 33.1 ms；2 mm 各向同性；MB 8 |
| 功能定位任务 | HCP 工作记忆任务；faces、places、tools、body parts；0-back 与 2-back |
| 标准化流程 | HCP minimal preprocessing 与 MSM 表面配准 |
| 发布表面空间 | `32k_fs_LR` 与 `164k_fsavg_LR` |

### 实际文件组成

包内共有 17 个文件，其中 14 个为 CIFTI 图像。下表的两种空间后缀分别是 `32k_fs_LR` 和 `164k_fsavg_LR`。

| 文件模式 | 每种空间数量 | 内容 |
| --- | ---: | --- |
| `HCP-YA_FFA-indiv.{space}.dlabel.nii` | 1 | 1,053 位参与者各自的 FFA 标签图；图像内保留参与者标识 |
| `HCP-YA_FFA-prob.{space}.dscalar.nii` | 1 | 各 FFA 的连续概率图 |
| `HCP-YA_FFA-MPM_thr-{0,10,25,50}.{space}.dlabel.nii` | 4 | 0%、10%、25%、50% 门槛的最大概率分区 |
| `HCP-YA_FFA-split.{space}.dlabel.nii` | 1 | 依据 FFA 概率，将 HCP MMP 中的 FFC 分为 pFus 与 mFus |
| `subject_id.txt` | 全包 1 份 | 参与者 ID，每行一个，共 1,053 行 |
| `group_id.csv` | 全包 1 份 | `lh`、`rh` 两列，记录每个参与者各半球的拓扑分组 |
| `readme.pdf` | 全包 1 份 | 研究方法、文件解释、标签与图例 |

概率图按该 ROI 可识别的参与者计算其在某个顶点出现的比例。`MPM` 将顶点分配给最大概率对应的 FFA；阈值决定纳入分区的最低最大概率。

### 实际 CIFTI 维度与标签

以下 shape 采用 NiBabel 读取后的 `(map, brainordinate)` 轴顺序。

| 图像 | `32k_fs_LR` | `164k_fsavg_LR` | 第 0 轴含义 |
| --- | --- | --- | --- |
| `indiv` | `(1053, 59412)` | `(1053, 327684)` | 参与者，`LabelAxis` |
| `prob` | `(2, 59412)` | `(2, 327684)` | `pFus-faces`、`mFus-faces`，`ScalarAxis` |
| 每个 `MPM` | `(1, 59412)` | `(1, 327684)` | 群体标签图，`LabelAxis` |
| `split` | `(1, 59412)` | `(1, 327684)` | FFC 二分标签图，`LabelAxis` |

概率文件的两个 map 按 `pFus-faces`、`mFus-faces` 排列，每个 map 均通过脑模型轴同时表达两个半球。标签文件的实际键值及内嵌名称为：

| 标签键 | 内嵌名称 | 含义 | RGBA |
| ---: | --- | --- | --- |
| 0 | `???` | 透明背景 | `(1, 1, 1, 0)` |
| 1 | `R_pFus-faces` | 右侧 FFA-1 | `(0, 1, 0, 1)` |
| 2 | `R_mFus-faces` | 右侧 FFA-2 | `(0, 0, 1, 1)` |
| 3 | `L_pFus-faces` | 左侧 FFA-1 | `(0, 1, 0, 1)` |
| 4 | `L_mFus-faces` | 左侧 FFA-2 | `(0, 0, 1, 1)` |

`32k_fs_LR` 脑模型轴纳入左半球 29,696 个、右半球 29,716 个顶点，共 59,412 个 brainordinates；完整网格元数据为每半球 32,492 个顶点。`164k_fsavg_LR` 纳入每半球全部 163,842 个顶点，双侧合计 327,684。按 brain model 保存的顶点索引定位数据，可与相应完整表面网格建立对应。

个体文件可通过 `img.dataobj[participant_index]` 按参与者读取。其概率与标签均按 CIFTI 轴解释，表面几何由所选参考网格提供。

### 拓扑分组

`group_id.csv` 的每一行与 `subject_id.txt` 的参与者顺序对应。

| 分组编号 | 名称 | 定义 |
| ---: | --- | --- |
| `0` | `single` | 某半球识别到一个 FFA |
| `1` | `continuous` | 两个 FFA 空间相接，可结合沟回地标区分 |
| `2` | `separate` | 两个 FFA 之间存在皮层间隔 |

本包 `group_id.csv` 的实际分组计数如下，计数单位为半球：

| 分组 | 左半球 | 右半球 | 合计 |
| --- | ---: | ---: | ---: |
| `single` | 46 | 50 | 96 |
| `continuous` | 247 | 315 | 562 |
| `separate` | 760 | 688 | 1,448 |
| **合计** | **1,053** | **1,053** | **2,106** |

个体标签图、概率图、`MPM` 与 `FFC split` 分别对应个体定义、群体分布、互斥群体分区和 HCP FFC 内部细分，分析时按研究问题选择。

## MT_atlas：人类视觉运动复合区图谱

### 研究内容与构建空间

原始说明：`MT_atlas/ReadMe.txt`。资源提供人类视觉运动复合区 `hMT+` 的概率图。对应研究利用 509 位参与者的功能定位数据，正式论文见 [Huang et al., 2019](https://doi.org/10.1002/hbm.24610)。论文的标准图谱构建采用高 temporal SNR 子样本，左半球 239 人、右半球 241 人。

包内说明明确记载：表面图谱位于 FreeSurfer average brain；体积版本通过 `mri_surf2vol --fillribbon` 从表面版本投影至 MNI152 空间。`snrH`、`snrL` 在下表中保留为原始文件的版本标记。

### 阈值与文件版本

`thr3`、`thr4`、`thr5`、`thr6` 对应原始定位使用的 `p = 10^-3`、`10^-4`、`10^-5`、`10^-6`。导出的概率图仍按概率标度读取；进一步定义 ROI 时可单独设置概率门槛。

| 文件版本 | 个体定位统计阈值 | 表面文件 | 体积文件 |
| --- | --- | --- | --- |
| `thr3_snrH` | `p = 1e-3` | `pm_thr3_snrH_lh.mgz`、`pm_thr3_snrH_rh.mgz` | `pm_thr3_snrH.nii.gz` |
| `thr3_snrL` | `p = 1e-3` | `pm_thr3_snrL_lh.mgz`、`pm_thr3_snrL_rh.mgz` | `pm_thr3_snrL.nii.gz` |
| `thr4_snrH` | `p = 1e-4` | `pm_thr4_snrH_lh.mgz`、`pm_thr4_snrH_rh.mgz` | `pm_thr4_snrH.nii.gz` |
| `thr5_snrH` | `p = 1e-5` | `pm_thr5_snrH_lh.mgz`、`pm_thr5_snrH_rh.mgz` | `pm_thr5_snrH.nii.gz` |
| `thr6_snrH` | `p = 1e-6` | `pm_thr6_snrH_lh.mgz`、`pm_thr6_snrH_rh.mgz` | `pm_thr6_snrH.nii.gz` |

表面文件位于 `MT_atlas/surface/`，共 10 个；体积文件位于 `MT_atlas/volume/`，共 5 个。每份体积图表示对应版本的双侧空间概率分布，表面文件按左右半球分别存储。

所有表面文件 shape 为 `(163842, 1, 1)`，每份对应一个半球的 163,842 个 fsaverage 顶点，存储为大端 `float32`。体积文件 shape 为 `(91, 109, 91)`，体素大小 2 mm，`float32`、`LAS`，有效 affine 与下文公共网格一致。实际图像采用 `0-1` 概率标度。

## 空间与数据读取

### 体积图像的公共网格

BAA-OR、BAA-WM 和 MT_atlas 的体积图像共享以下有效空间网格：

| 参数 | 实际读取值 |
| --- | --- |
| 空间数组大小 | `(91, 109, 91)`；BAA-OR 的多 ROI PRM 另带通道维 |
| 体素大小 | `(2.0, 2.0, 2.0)` mm |
| 轴向编码 | `LAS`，由 NIfTI 的有效 affine 计算 |
| 体素体积 | 8 mm^3 |

```text
affine =
[[-2,  0,  0,   90],
 [ 0,  2,  0, -126],
 [ 0,  0,  2,  -72],
 [ 0,  0,  0,    1]]
```

这里的 affine 将零起始体素索引 `(i, j, k)` 转换为物理坐标。图像数组、affine 与原始标准化流程共同确定空间对应关系；跨数据应用时同时记录所用参考模板和配准变换。

BAA-OR 的表面 NIfTI 与 MT 的表面 MGZ 均以 `(163842, 1, 1)` 数组保存单半球顶点数据。将第一维与 fsaverage 的顶点编号配对读取，表面距离和面积使用相应网格的几何信息计算。

### 安装 Python 读取依赖

```bash
python -m pip install numpy nibabel
```

NiBabel 可用于读取本仓库的 NIfTI、FreeSurfer MGH/MGZ 和 CIFTI。相关接口见 [NiBabel CIFTI 文档](https://nipy.org/nibabel/reference/nibabel.cifti2.html)。

以下四组示例已基于包内实际数据运行，验证环境为 Python 3.12.6、NiBabel 5.4.2、NumPy 2.4.6。

### 示例一：读取 BAA-OR 4D 概率图

以下代码在仓库根目录运行，假设已按前文解压到 `extracted/`。

```python
from pathlib import Path
import nibabel as nib
import numpy as np

path = Path("extracted/BAA-OR/face/volume/BAA-OR-FvO-PRM-2mm.nii.gz")
img = nib.load(path)
print("shape:", img.shape)
print("voxel size (mm):", img.header.get_zooms()[:3])
print("axis codes:", nib.aff2axcodes(img.affine))
print("stored dtype:", img.get_data_dtype())

region_names = [
    "rOFA", "lOFA", "rpFus", "lpFus", "raFus", "laFus",
    "rpcSTS", "lpcSTS", "rpSTS", "lpSTS", "raSTS", "laSTS",
]
assert img.shape == (91, 109, 91, len(region_names))
right_ofa = np.asarray(img.dataobj[..., region_names.index("rOFA")], dtype=np.float32)
print("rOFA probability range:", float(right_ofa.min()), float(right_ofa.max()))
```

通过 `img.dataobj[..., index]` 读取选定通道，可按分析需要控制内存使用。

### 示例二：导出带明确阈值的 rOFA 二值 ROI

本示例选择 `p >= 0.25`，作为透明、可修改的操作示例。具体研究按预先制定的分析方案选择概率阈值。

```python
from pathlib import Path
import nibabel as nib
import numpy as np

source = Path("extracted/BAA-OR/face/volume/BAA-OR-FvO-PRM-2mm.nii.gz")
img = nib.load(source)
probability = np.asarray(img.dataobj[..., 0], dtype=np.float32)  # rOFA
threshold = 0.25
mask = (probability >= threshold).astype(np.uint8)

header = img.header.copy()
header.set_data_dtype(np.uint8)
roi = nib.Nifti1Image(mask, img.affine, header=header)
output = Path("derived/rOFA_prob-ge-025.nii.gz")
output.parent.mkdir(parents=True, exist_ok=True)
nib.save(roi, output)

voxel_count = int(mask.sum())
voxel_volume = float(abs(np.linalg.det(img.affine[:3, :3])))
print("output:", output)
print("voxels:", voxel_count)
print("volume (mm^3):", round(voxel_count * voxel_volume, 3))
```

对本包的 rOFA PRM 使用上述 25% 阈值，输出为 512 个体素、4,096 mm^3。

独立 ROI 概率图，例如 BAA-WM 的 `lMFG-PRM`，可直接对三维数组应用同样的阈值操作。输出建议放入单独的 `derived/` 目录，并在名称中保留脑区、空间和阈值信息。

### 示例三：读取 HCP-FFA 的 CIFTI 标签与空间轴

```python
from pathlib import Path
import nibabel as nib
import numpy as np

path = Path("extracted/HCP-FFA/HCP-YA_FFA-MPM_thr-25.32k_fs_LR.dlabel.nii")
img = nib.load(path)
label_axis = img.header.get_axis(0)
brain_axis = img.header.get_axis(1)
labels = np.asarray(img.dataobj[0])

print("shape:", img.shape)
print("axes:", type(label_axis).__name__, type(brain_axis).__name__)
for key, (name, rgba) in sorted(label_axis.label[0].items()):
    print("label:", key, name, "rgba:", rgba)
for structure, selection, model in brain_axis.iter_structures():
    print(structure, "mapped vertices:", len(model),
          "full mesh vertices:", brain_axis.nvertices[structure])
print("present label values:", np.unique(labels))
```

CIFTI 的脑模型轴保存左右半球与实际顶点索引的映射。编程时按该轴确定数组列与表面网格的对应；读取 individual 文件时通过第 0 轴名称与 `subject_id.txt` 识别参与者。

### 示例四：读取 fsaverage 顶点 NIfTI 与 MGZ

```python
from pathlib import Path
import nibabel as nib
import numpy as np

paths = [
    Path("extracted/BAA-OR/face/surface/BAA-OR-FvO-lOFA-PRM-fsaverage.nii.gz"),
    Path("extracted/MT_atlas/surface/pm_thr3_snrH_lh.mgz"),
]
for path in paths:
    img = nib.load(path)
    probability = np.asarray(img.dataobj, dtype=np.float32).reshape(-1)
    assert probability.shape == (163842,)
    print(path.name, "vertices:", probability.size,
          "probability range:", float(probability.min()), float(probability.max()))
```

本例两份文件均为左半球 fsaverage 顶点数据。更换文件时，将半球、网格密度与参考表面同步记录。

### 配准、插值与表面数据

| 数据类型 | 对应处理原则 |
| --- | --- |
| 连续概率图 | 保持 `0-1` 标度；同一标准空间内调整采样网格时可采用线性插值，再应用分析概率阈值 |
| 离散标签图、二值 ROI | 采用最近邻插值，保留原始类别编码 |
| `RMM` / `RVM` | 按连续值处理，保留其 PSC 相关单位和原始数值标度 |
| fsaverage 表面 NIfTI / MGZ | 将数组元素解释为指定半球的顶点数据，并匹配相同 fsaverage 网格 |
| HCP CIFTI | 同时匹配表面空间、网格密度、brain model 和顶点映射 |

同一标准空间中的网格重采样可参考 [NiBabel `resample_from_to`](https://nipy.org/nibabel/reference/nibabel.processing.html#resample-from-to)。跨标准空间或进入个体原生空间时，先确定实际配准变换，再将该变换应用到相应图谱。采集分辨率、发布图谱分辨率和表面网格密度分别记录。

## 分析与报告约定

为了便于复现，研究记录建议包含以下项目：

1. **图谱与文件**：压缩包名称、所用包内完整路径，以及对应原始论文。
2. **脑区定义**：任务对比、半球、ROI 名称、4D 通道索引或标签键值。
3. **图像语义**：`PAM`、`PRM`、`MPRM/MPM`、`RMM`、`RVM`、individual 或 `FFC split`。
4. **阈值**：区分原始个体定位的统计阈值、发布最大概率分区的门槛和本次分析另设的概率阈值。
5. **空间信息**：参考模板、采样网格或表面密度、配准变换及插值方法。
6. **ROI 汇总方式**：二值掩膜或概率加权、体素数或顶点数，以及所采用的信号汇总规则。
7. **派生数据命名**：保留来源、脑区、空间与阈值，单独存放派生结果。

BAA-OR 的 `aFus/pFus`、HCP-FFA 的 `mFus/pFus`、HCP MMP 的 `FFC` 均保留各自研究的命名与定位依据；进行跨图谱比较时，将这些定义作为方法信息一并记录。

## 原始资料与参考文献

### 包内原始资料

| 数据包 | 原始说明路径 | 阅读定位 |
| --- | --- | --- |
| BAA-OR | `BAA-OR/BAA-OR-Design Paradigm and Data Acquisition.pdf` | 第 1-2 页：范式与采集；第 2-4 页：处理与图像构建；第 5-8 页：文件和通道顺序 |
| BAA-WM | `BAA-WM/BAA-WM Design Paradigm and Data Acquisition.pdf` | 第 1-2 页：范式与采集；第 2-4 页：图谱构建；第 4-5 页：文件定义 |
| HCP-FFA | `HCP-FFA/readme.pdf` | 第 1-4 页：样本、定位、概率图与拓扑分组；第 5-6 页：文件；第 7-10 页：原始图例 |
| MT_atlas | `MT_atlas/ReadMe.txt` | 统计阈值、表面与体积空间、投影方法及作者联系方式 |

### 原始研究

1. **BAA-OR / 面孔图谱**：Zhen, Z., Yang, Z., Huang, L., Kong, X.-Z., Wang, X., Dang, X., Huang, Y., Song, Y., & Liu, J. (2015). Quantifying interindividual variability and asymmetry of face-selective regions: A probabilistic functional atlas. *NeuroImage, 113*, 13-25. [DOI: 10.1016/j.neuroimage.2015.03.010](https://doi.org/10.1016/j.neuroimage.2015.03.010).
2. **BAA-OR / 场景图谱**：Zhen, Z., Kong, X.-Z., Huang, L., Yang, Z., Wang, X., Hao, X., Huang, T., Song, Y., & Liu, J. (2017). Quantifying the variability of scene-selective regions: Interindividual, interhemispheric, and sex differences. *Human Brain Mapping, 38*, 2260-2275. [DOI: 10.1002/hbm.23519](https://doi.org/10.1002/hbm.23519)。
3. **BAA-WM**：Chen, C., Zhang, Y., Zhen, Z., Song, Y., Hu, S., & Liu, J. (2021). Quantifying the variability of neural activation in working memory: A functional probabilistic atlas. *NeuroImage, 239*, 118301. [DOI: 10.1016/j.neuroimage.2021.118301](https://doi.org/10.1016/j.neuroimage.2021.118301).
4. **HCP-FFA**：Chen, X., Liu, X., Parker, B. J., Zhen, Z., & Weiner, K. S. (2023). Functionally and structurally distinct fusiform face area(s) in over 1000 participants. *NeuroImage, 265*, 119765. [DOI: 10.1016/j.neuroimage.2022.119765](https://doi.org/10.1016/j.neuroimage.2022.119765)。包内说明同时保留了该研究的 [2022 年预印本引用入口](https://doi.org/10.1101/2022.04.08.487562)。作者发布的相关资源另见 [BALSA study x87Zk](https://balsa.wustl.edu/study/x87Zk)。
5. **MT_atlas**：Huang, T., Chen, X., Jiang, J., Zhen, Z., & Liu, J. (2019). A probabilistic atlas of the human motion complex built from large-scale functional localizer data. *Human Brain Mapping, 40*, 3475-3487. [DOI: 10.1002/hbm.24610](https://doi.org/10.1002/hbm.24610).

### 学术归属与使用

本仓库整理第三方原始研究资源与使用说明，图谱、实验数据和配套文档的学术归属保留给原作者及来源项目。使用图谱发表成果时引用相应原始研究；使用、共享与再分发遵循各来源项目的适用条款。HCP 衍生资源同时遵循 HCP 对数据使用与致谢的相关要求。
