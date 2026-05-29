# Biuty Merchant Dashboard 项目梳理

## 当前文件

- 原始文件：`/Users/anyita/Desktop/Biuty/ToB Dashboard/biuty — Merchant.html`
- 当前工作副本：`/Users/anyita/Documents/Codex/2026-05-14/files-mentioned-by-the-user-biuty/index.html`
- 类型：单文件 HTML 原型，CSS 和 JavaScript 都内嵌在同一个文件里。
- 规模：约 2,466 行，181 KB。
- 外部依赖：Google Fonts `Plus Jakarta Sans`。

## 产品定位

这是 Biuty 的商家端 Dashboard 原型，面向美容/医美商家日常运营。当前示例商家是 `Glow Studio KL`，核心场景包括预约、券核销、订单、客户档案、商家 listing 管理、经营分析，以及 AI Skin Scan。

## 信息架构

全局布局由三部分组成：

- Top bar：品牌、Merchant 标签、门店状态、通知、头像。
- Sidebar：主导航，按 Overview、Clients、Insights 分组。
- Main：不同页面通过 `.page` 和 `nav(page)` 切换显示。

主页面共有 8 个：

- `Dashboard`：今日收入、预约数、券核销数、活跃客户、今日排程、待核销券、本周收入图。
- `Redeem Voucher`：扫码/输入券码核销，显示今日核销和历史核销。
- `Appointments`：日/周/月视图、员工过滤、今日统计、预约详情弹窗、Skin Scan 入口。
- `Orders`：Treatment Orders 和 Product Orders 两个 tab，支持从订单直接核销券。
- `Client Profiles`：客户列表、客户详情、Overview/History/Spend/Notes tabs、SMS/Email/Call/Book/Edit 操作。
- `My Listings`：商家发布的 treatment/product listings，支持展开编辑、草稿/发布状态、新建 listing。
- `Analytics`：月度指标、项目收入占比、Top clients。
- `Settings`：门店信息、通知设置、账号信息。

## 主要弹窗

当前定义了 12 个 modal：

- `modal-appt`：新建预约。
- `modal-client`：新建客户档案。
- `modal-new-listing`：新建 treatment/product listing。
- `modal-appt-detail`：预约详情。
- `modal-sms`：给客户发 SMS。
- `modal-email`：给客户发 Email。
- `modal-topup`：客户储值/credit top-up。
- `modal-add-history`：新增治疗记录。
- `modal-skin-scan-select`：选择 Skin Scan 客户。
- `modal-skin-scan-camera`：模拟拍摄/识别人脸。
- `modal-skin-scan-analysing`：模拟 AI 分析中。
- `modal-scan-report`：Skin Scan 报告结果。

## Mock 数据

JavaScript 中有两组核心 mock 数据：

- `VOUCHERS`：券码和治疗/客户/有效期/使用状态。
  - `HF-4829` Mei Ying / HydraFacial Deluxe。
  - `UL-7731` Aisha Rahman / Ultherapy。
  - `BT-9901` Linda Tan / Botox Full Face。
  - `TH-2201` Sarah Lim / Thermage FLX，已使用。
  - `IP-0034` Priya Nair / IPL Photofacial，剩余 2/3。
- `CLIENTS`：6 个客户档案，包含 initials、联系方式、生日、Fitzpatrick type、过敏、skin concerns、历史治疗和 active voucher。

## 已有交互

- `nav(page)`：切换左侧导航和主页面。
- `openModal(id)` / `closeModal(id)`：打开和关闭弹窗。
- `showToast(msg)`：底部 toast。
- `switchTab`：订单页 treatment/product tab。
- `switchApptView`：预约页 day/week/month。
- `switchClientTab`：客户详情 overview/history/spend/notes。
- `triggerRedeem` / `doRedeem` / `confirmRedeem` / `cancelRedeem`：券核销流程。
- `selectClient` / `toggleVIP`：客户详情数据切换和 VIP 标记。
- `toggleListing` / `switchListingTab` / `toggleNewListingType`：listing 展开、tab、新建类型切换。
- `fillSmsTemplate`：短信模板填充。
- `filterScanClients` / `selectScanClient` / `simulateFaceDetected` / `switchScanTab`：Skin Scan 三步模拟流程。

## 当前明显待修点

- Orders 里有按钮调用 `openModal('modal-order-detail')`，但文件里没有定义 `modal-order-detail`。
- Client Profiles 里 Edit 按钮调用 `openModal('modal-client-edit')`，但文件里没有定义 `modal-client-edit`。
- `openModal(id)` 没有空值保护，点击上述缺失 modal 会直接报错。
- Analytics 的 `Revenue by Treatment` 金额显示缺了前缀和首位数字，例如现在是 `,680 · 34%`，看起来应该是类似 `$9,680 · 34%`。
- `selectScanClient` 依赖隐式全局 `event.currentTarget`，在某些浏览器或后续重构时不稳定。
- 页面使用 `<meta name="viewport" content="width=1024">`，整体更像 iPad/桌面原型，不是响应式移动版。
- 目前大量 inline style 和 inline `onclick`，适合快速原型，但后续维护建议拆成结构、样式、数据和逻辑模块。

## 后续可以继续做的方向

- 先补齐缺失的 `Order Detail` 和 `Client Edit` modal，避免点击时报错。
- 修复 Analytics 金额显示。
- 给 `openModal` / `closeModal` 加基础容错。
- 把 mock 数据抽出来，统一驱动 dashboard、orders、clients、redeem，减少重复硬编码。
- 把当前单文件入口继续拆成 `styles.css`、`app.js`，或者迁移成 React/Vue/Svelte 原型。
- 做真实响应式适配，尤其是 appointment、client detail、listing editor 这几个复杂页面。
- 用浏览器跑一轮交互 smoke test，确认每个导航、tab、modal 和主要按钮都能正常工作。
