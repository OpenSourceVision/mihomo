# Mihomo P5分流配置文件说明

## 概述

Mihomo (Clash) 代理配置文件，该配置文件包含了完整的代理分组和分流规则设置，适用于全球网络访问的智能路由。

## 地区分组

配置文件包含以下地区的自动选择代理组：

### 🇺🇸 美国 (US AUTO)
- **类型**: url-test (自动测试)
- **匹配规则**: `(?i)美国|USA|🇺🇸`
- **测试间隔**: 300秒
- **用途**: 自动选择最快的美国节点

### 🇬🇧 英国 (UK AUTO)
- **类型**: url-test (自动测试)
- **匹配规则**: `(?i)英国|UK|Britain|🇬🇧`
- **测试间隔**: 300秒
- **用途**: 自动选择最快的英国节点

### 🇫🇷 法国 (FR AUTO)
- **类型**: url-test (自动测试)
- **匹配规则**: `(?i)法国|France|FR|🇫🇷`
- **测试间隔**: 300秒
- **用途**: 自动选择最快的法国节点

### 🇷🇺 俄罗斯 (RU AUTO)
- **类型**: url-test (自动测试)
- **匹配规则**: `(?i)俄罗斯|Russia|RU|🇷🇺`
- **测试间隔**: 300秒
- **用途**: 自动选择最快的俄罗斯节点

### 🇨🇳 中国直连 (CN DIRECT)
- **类型**: select (手动选择)
- **选项**: DIRECT, MANUAL
- **用途**: 中国大陆网站直连或手动选择

## 功能分组

### MANUAL (手动选择)
- **功能**: 全局手动选择代理
- **默认选项**: US AUTO, UK AUTO, FR AUTO, RU AUTO
- **用途**: 需要手动控制时使用

### GLOBAL (全球默认)
- **功能**: 全局流量的默认分流
- **包含所有代理组**: 涵盖所有地区和功能分组
- **用途**: 未匹配其他规则的流量使用

### 🌐 FALLBACK (漏网之鱼)
- **功能**: 处理所有未匹配规则的流量
- **默认选择**: US AUTO (优先美国节点)
- **备选**: MANUAL
- **用途**: 作为最终兜底规则，确保所有流量都能正常代理

## 应用专用分组

### 🤖 AI 服务
- **包含服务**: OpenAI, Claude, Bard, Bing, Copilot
- **默认选择**: US AUTO (优先美国节点)
- **备选**: UK AUTO, FR AUTO, MANUAL
- **特点**: 专门为 AI 服务优化的代理选择

### 💼 Microsoft 服务
- **默认选择**: CN DIRECT (优先直连)
- **备选**: US AUTO, UK AUTO, FR AUTO, MANUAL
- **用途**: Microsoft 相关服务的访问优化

### 💻 GitHub
- **默认选择**: US AUTO (优先美国节点)
- **备选**: UK AUTO, FR AUTO, MANUAL
- **用途**: GitHub 代码托管平台访问

### 📱 Telegram
- **默认选择**: US AUTO (优先美国节点)
- **备选**: UK AUTO, FR AUTO, RU AUTO, MANUAL
- **用途**: Telegram 即时通讯

### 📺 YouTube
- **默认选择**: RU AUTO (优先俄罗斯)
- **备选**: US AUTO, UK AUTO, FR AUTO, MANUAL
- **用途**: YouTube 视频流媒体

### 🔍 Google 服务
- **默认选择**: US AUTO (优先美国节点)
- **备选**: UK AUTO, FR AUTO, MANUAL
- **用途**: Google 搜索及相关服务

### ☁️ Cloudflare
- **默认选择**: US AUTO (优先美国节点)
- **备选**: CN DIRECT, UK AUTO, FR AUTO, MANUAL
- **用途**: Cloudflare CDN 服务优化

## 分流规则详解

### 优先级排序 (从高到低)

1. **私有网络** (`private`) → `DIRECT`
   - 本地网络、内网地址直连

2. **中国大陆** (`cn_domain`, `cn_ip`) → `CN DIRECT`
   - 中国大陆网站和 IP 地址直连

3. **专用服务分流**:
   - `microsoft` → `Microsoft`
   - `github` → `GitHub`
   - `cloudflare` → `Cloudflare`

4. **AI 服务分流**:
   - `bing` → `AI`
   - `copilot` → `AI`
   - `bard` → `AI`
   - `openai` → `AI`
   - `claude` → `AI`

5. **媒体和通讯**:
   - `youtube` → `YouTube`
   - `google_domain`, `google_ip` → `Google`
   - `telegram_domain`, `telegram_ip` → `Telegram`

6. **海外网站** (`geolocation-!cn`) → `FALLBACK`
   - 非中国大陆的网站

7. **默认规则** (`MATCH`) → `FALLBACK`
   - 所有未匹配的流量

## 配置特点

### 智能过滤
- **排除规则**: `(?i)GB|Traffic|Expire|Premium|频道|订阅|ISP|流量|到期|重置`
- **作用**: 自动排除流量统计、过期提醒等非实际代理节点

### 自动化程度高
- 所有地区组都使用 `url-test` 类型
- 5分钟自动测试间隔，确保连接质量
- 智能选择最优节点

### 规则覆盖全面
- 涵盖主流互联网服务
- 专门针对 AI 服务优化
- 中国大陆网站智能直连
- 海外服务合理分流

### 美国节点优先策略
- AI 服务、GitHub、Google、Cloudflare 等关键服务优先使用美国节点
- 确保访问稳定性和速度
- 降低服务不可用的风险

## 更新内容

### 主要变更
1. **AI 服务**: 默认改为 US AUTO (美国节点优先)
2. **GitHub**: 默认改为 US AUTO (美国节点优先)
3. **Google 服务**: 默认改为 US AUTO (美国节点优先)
4. **Cloudflare**: 默认改为 US AUTO (美国节点优先)
5. **移除游戏规则**: 删除了 Steam 相关的分流规则
6. **新增漏网之鱼组**: 添加 FALLBACK 组，默认使用 US AUTO
7. **规则优化**: 所有未匹配规则的流量统一使用 FALLBACK 组

### 优势
- **稳定性提升**: 美国节点通常具有更好的稳定性和速度
- **服务兼容性**: 大多数国际服务在美国都有良好支持
- **简化管理**: 统一的漏网之鱼策略，减少规则复杂度

## 使用建议

1. **日常使用**: 直接使用各个专用分组，无需手动切换
2. **特殊需求**: 使用 MANUAL 组进行精确控制
3. **性能优化**: 配置会自动选择最快节点，无需频繁调整
4. **故障排除**: 如某个服务异常，可临时切换到 MANUAL 手动选择节点
5. **新服务**: 未配置专门规则的新服务会自动使用 FALLBACK 组的美国节点