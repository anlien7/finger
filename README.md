# finger

- 为现有的finger补充国内常见 Web 指纹库。

- 已经包含的指纹库内容为finger.json。为了方便解决统一合并，新加入的指纹将添加finger_v1.json。Pr的时候直接拉取仓库将finger_v1.json添加到团队finger.json文件里面即可。

- 后续发展指纹库方向：httpx 内置技术栈识别基于例如：EHole 棱洞、TideFinger、Xray、Goby 内置指纹库等。将其各种格式转化为 `httpx` 所用。

- 后续具体做法：**编写一个“指纹转换脚本”**，将各个开源项目的指纹提取出来，统一转换为 `httpx`所定义的格式。

- 添加重点为：电力系统、国内的政企、高校、红蓝对抗场景，大量**国产专属系统**（如：泛微/致远/通达 OA、用友/金蝶 ERP、深信服/网御星云/天融信安全设备、海康威视/大华监控、宝塔面板等）。

## 使用方法

httpx 0.0.7 以上版本（启用 `-tech-detect`）支持 `-custom-fingerprint-file`，将 `finger.json` 与默认指纹库合并使用：

```bash
httpx -l urls.txt -td -cff finger.json
```

只想使用本仓库指纹（不加载内置库），可使用 `wappalyzergo.NewFromFile(path, loadEmbedded=false, supersede=false)` 进行二次集成。

## 文件格式

`finger.json` 遵循 wappalyzergo 的官方 schema，结构为：

```json
{
  "apps": {
    "AppName": {
      "cats": [1],
      "description": "...",
      "website": "https://...",
      "icon": "AppName.png",
      "headers": { "header-name": "regex" },
      "cookies": { "cookie-name": "regex" },
      "html":     ["regex", "regex"],
      "scriptSrc":["regex"],
      "meta":     { "meta-name": ["regex"] },
      "dom":      { "css-selector": { "exists": "" } },
      "js":       { "window.var": "regex" },
      "url":      ["regex"],
      "implies":  ["OtherApp"]
    }
  }
}
```

关键约定：

- 所有匹配字段是 Go `regexp` 语法的正则，JSON 内需要双反斜杠转义（如 `\\d`、`\\.`）。
- 在正则末尾追加 `\\;version:\\1` 可用捕获组提取版本号。
- 在正则末尾追加 `\\;confidence:50` 可降低置信度（默认 100），多条 50 分会累加；置信度 0 等同于不构成证据，不要使用。
- `cats` 数字含义见 wappalyzergo 仓库的 [categories_data.json](https://github.com/projectdiscovery/wappalyzergo/blob/main/categories_data.json)（如 1=CMS，16=Security，22=Web servers，30=Webmail，31=CDN，37=Network devices，46=Remote access，50=DMS，65=Load balancers）。
- `implies` 用来联动出其他组件，例如检出 `ThinkPHP` 时自动补 `PHP`。
- `headers`/`cookies`/`meta` 的 key 一律小写。

# 覆盖产品

### CMS / 建站

**历史收录:**MetInfo、Empire CMS（帝国 CMS）、PbootCMS、74CMS（齐博）、苹果 CMS（maccms）、DouPHP、CmsEasy、SDCMS、JeecgBoot、JeeSite、RuoYi（若依）、JPress、JeeCMS、OneThink、HiShop、HDWiki、Z-BlogPHP、Typecho、Emlog、Discuz! Q、Halo。

**新增收录 ：** 

### 论坛 / 社区

**历史收录:**WeCenter、Tipask。

**新增收录 ：** 

### OA / ERP

**历史收录:**通达 OA、致远 OA、泛微 e-cology、泛微 e-office、泛微 e-mobile、蓝凌 OA、华天动力 OA、金和 OA、用友 NC/NCC、用友 U8、金蝶 EAS、金蝶云·星空、万户 OA。

**新增收录 ：** 

### 邮件系统

**历史收录:** Coremail、TurboMail、U-Mail、WinWebMail、Magic Winmail、亿邮邮件系统。

**新增收录 ：** 

### 网络 / 安全设备

**历史收录:** 深信服 SSL VPN/AC/AF/EDR/WAF、奇安信天眼、奇安信网神 SecGate、绿盟 WAF/IPS、山石网科 Hillstone、天融信 NGFW/TopApp-LB、启明星辰天清汉马、安恒明御 WAF、安恒玄武盾、网御星云 SecFox、网康 NGFW、华为 USG、华为 eSpace、H3C、锐捷、海康威视、大华、宇视。

**新增收录 ：** 

### 堡垒机 / 运维

**历史收录:** JumpServer、齐治堡垒机、行云堡垒。

**新增收录 ：** 

### 运维面板

**历史收录:** 宝塔面板、aaPanel、1Panel、小皮面板（phpStudy）。

**新增收录 ：** 

### 编辑器 / 报表 / BI

**历史收录: **KindEditor、UEditor、FineReport、FineBI、Smartbi、永洪 BI、DataEase。

**新增收录 ：** 

### 协同 / SaaS

**历史收录:** 钉钉、企业微信、飞书 / Lark、小鹅通、石墨文档、金山文档、腾讯文档、Teambition、Tower。

**新增收录 ：** 

### 监控 / 测试 / 知识库

**历史收录:** Nightingale（夜莺）、MeterSphere、MaxKB、ShowDoc、YApi、Apifox、Eolinker。

**新增收录 ：** 

### Web 服务器 / 容器

**历史收录:** Tengine、OpenResty。

**新增收录 ：** 

### CDN / WAF

**历史收录:** 加速乐（Jiasule）、ChinaCache、白山云 CDN、又拍云 CDN、七牛云 CDN、UCloud CDN、京东云 CDN、腾讯云 EdgeOne、阿里云 WAF、腾讯云 WAF、百度云 WAF、宝塔 WAF、安全狗、云锁、知道创宇创宇盾。

**新增收录 ：** 

### 代码托管

**历史收录:** Gitea（中文部署）、Gogs、Coding。

**新增收录 ：** 

### 其他

**历史收录** PaddlePaddle、EasyImage、ChatGLM-Web、WordPress 国产主题。

**新增收录 ：** 

# 贡献

添加新指纹前已经做好以下准备

1. 至少包含一个特异性较强的字段（独有 cookie/header、独有 script 路径、独有 favicon hash 替代字段、独有标题片段）；通用 cookie（如 `PHPSESSID`、`JSESSIONID`、`_csrf`）
2. 本地至少跑一次 JSON 校验，附 1-2 个真实样本的命中验证。
3. 多条 `html`/`scriptSrc`，尽量保证独立，搭配组合证据。
4. 正则尽量避免 `.*` 起手，必要的边界用 `^`/`$` 限制。
5. 已经筛查出不属于 wappalyzergo 已内置组件。

# 更新日志

后续将会在这里将每天在此记录新增的finger

例：

```
## 更新日志
- [2026-08-10] 更新:
  - `CMS / 建站`: 新增织梦 CMS、XYCMS 等 15 个指纹。
  - `网络 / 安全设备`: 新增迪普科技、盈高科技 ASM 指纹。
```

