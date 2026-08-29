# 五大刊

金融顶刊阅读器。在手机上像刷小红书一样读 **JF / JFE / RFS / JFQA / RF**，而不是在聊天里一张张翻。

当前安装包：**[v2.1.1](https://github.com/linshp7-create/wudakan/releases/tag/v2.1.1)**（应用名「五大刊」，包名 `com.lin.wudakan`）。打开应用会从本仓库拉取最新目录；有新 APK 时应用内提示更新。

---

## 读什么

| 缩写 | 全称 |
| --- | --- |
| **JF** | The Journal of Finance |
| **JFE** | Journal of Financial Economics |
| **RFS** | The Review of Financial Studies |
| **JFQA** | Journal of Financial and Quantitative Analysis |
| **RF** | Review of Finance |

目录覆盖 **2026 年已出各期**，以及尚未编入正式刊的 **Early View / Advance Access**。正式出刊后，早报里的文章会归到对应那一期，不再出现在早报。

数据文件：[data/journal-data.json](data/journal-data.json)。工作日早上会核对出版社源，有新文或新一期就写进这个文件；打开 App 即可拉到，不必重装。

---

## 设计目标

1. **手机上读刊**，不在对话里排队出卡片。
2. **一屏很多张**，双列瀑布流。不是抖音式一页一张，也不是上图下文却占满整屏。
3. **两层阅读**：卡片只负责扫，点进去才看摘要、对话、笔记。
4. **中英分开**：英文标题用文字印在图上（不是生成图片）；下面只出中文译名，有翻译才显示。
5. **刊名用英文缩写**（JF、JFE、RFS、JFQA、RF），卡片上不写中文刊名。
6. **已读要记**：打开一篇即记已读；随机流里已读降权，但不隐藏。
7. **私人数据不出手机**：收藏、想法、浏览记录、API 密钥、对话、译文缓存都在本机。
8. **目录和安装包分开更新**：新文只改 JSON；改界面才发新 APK。

---

## 四个栏

### 期刊

杂志封面 → 选 2026 某一期 → 期内仍是双列卡片。适合按卷期把一期读完。

### 早报

还没进正式刊的新文。进了 bound issue 之后，从早报消失，出现在「期刊」对应那一期。

### 随机

把已收录论文洗成双列推荐流。未读优先，已读大约一成五会再出现；五刊交错，避免同一刊连刷。离开再进不会整墙重洗；「换一批」才重新洗。

### 我的

收藏、想法笔记、已读、浏览记录（今天 / 更早），以及本机的翻译 / 对话接口设置。

---

## 卡片和第二层

**卡片（上图下文）**

- 图上：英文原标题（HTML 文字叠在色块上，不另外生成图），左上角刊名缩写，已读标记，收藏心形。
- 图下：有中文译名才显示中文标题；没有则不重复英文。下一行是 `JF · 作者`。

**点进第二层**

| 页签 | 内容 |
| --- | --- |
| 摘要 | 原文摘要，可译成中文 |
| 对话 | 针对这一篇的问答 |
| 笔记 | 本机想法 / 评论 |

封面或卡片进入「摘要」；「问」进入「对话」。输入条钉在底部。

---

## 翻译和问答（可选）

在「我的」里填写自己的 **OpenAI 兼容接口**（Base URL、模型、密钥）。

- 密钥只存在手机 `localStorage`，**不会进本仓库、不会进 APK、不会上传**。
- 标题翻译做一次后缓存；卡片下次直接用中文。
- 摘要翻译、逐篇对话同样走这个接口，记录留在本机。

不填接口也能读刊、收藏、做笔记，只是没有翻译和问答。

---

## 目录如何更新

工作日 **8:00（Asia/Shanghai）** 扫一次五刊源：

- 新的 Early View / Advance Access 论文
- 新出的正式一期（volume / issue）
- 已从早报转入正式刊的篇目会从 `earlyView` 挪到对应 `journals[].issues[]`

有更新时：

1. 写入 [`data/journal-data.json`](data/journal-data.json)（`contentVersion` 改为当时时间）
2. 在维护者这边的对话里提示，可打开 App 拉新目录（无需重装）

没有新文则保持安静。周末不扫。不是出版社一上线立刻同步，而是下一个工作日早上。

App 启动时拉取该 JSON；仅当 `contentVersion` 比本地新才提示「目录已更新」。

---

## 安装

1. 打开 [Releases](https://github.com/linshp7-create/wudakan/releases/latest)，下载 `wudakan.apk`。
2. 若提示未知来源：系统设置 → 应用 → 特殊应用访问 → 安装未知应用，允许「五大刊」或你用来打开该文件的应用。
3. 已安装旧版时，应用内可检查 GitHub Release 更新。从很早的版本升级，有时需要先装一版带应用内更新的包（1.2.0 起）。

当前最新：**2.1.1**。

---

## 仓库里有什么

本仓库是 **内容与发布通道**，不是完整 Android 工程。

```
data/journal-data.json   五刊目录（App 启动时拉取）
README.md                本说明
```

安装包在 [Releases](https://github.com/linshp7-create/wudakan/releases)，不进 git 历史。

`journal-data.json` 结构概要：

```json
{
  "contentVersion": "2026-08-29T13:25:06+08:00",
  "appVersionMin": "1.6.0",
  "journals": [
    {
      "code": "JF",
      "full_name": "The Journal of Finance",
      "issues": [
        {
          "volume": "81",
          "issue": "4",
          "papers": [
            { "title": "", "authors": "", "abstract": "", "doi": "", "url": "", "pages": "" }
          ]
        }
      ]
    }
  ],
  "earlyView": [
    { "journal": "JF", "title": "", "authors": "", "abstract": "", "doi": "", "url": "", "posted_date": "", "status": "" }
  ]
}
```

改界面或逻辑才会发新的 APK / GitHub Release；只是多了几篇论文时，只推这个 JSON。

---

## 隐私

| 留在手机 | 不进 GitHub / 不进 APK |
| --- | --- |
| 收藏、已读、浏览记录 | API 密钥 |
| 笔记 / 想法 / 评论 | 对话记录 |
| 标题与摘要译文缓存 | 接口 Base URL 与模型名（只存在本机） |

本仓库公开的是期刊公开目录（标题、作者、摘要、DOI、链接），全部来自各刊官方页面或 RSS。

---

## 许可与来源

论文版权归各期刊与出版社。本项目只做个人阅读入口，不提供全文 PDF，不替代官方站点。请通过 DOI / 出版社链接获取正式版本。
