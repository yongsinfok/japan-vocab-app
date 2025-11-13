## 📄 前端架构文档 (Front-End Architecture)
**BMad Project Artifact: Architect (Winston) Output**

### **1. 技术栈综述 (Technology Stack Overview)**

| 类别 (Category) | 技术选型 (Chosen Technology) | 理由 (Rationale) |
| :--- | :--- | :--- |
| **基础框架** | **Next.js (基于 React)** | 提供 SSR/SSG 能力，有利于性能和 SEO。 |
| **PWA 实现** | **Next-PWA 插件 / Custom Service Worker** | 简化 Service Worker 注册和缓存策略配置。 |
| **离线数据** | **PouchDB (或 Dexie.js)** | 封装 IndexedDB，适用于存储大规模、结构化的用户数据（词汇、自定义卡片）。|
| **状态管理** | **Zustand / Jotai (轻量级库)** | 简洁、轻量，适用于 PWA 对性能的要求。|

### **2. 架构模块划分 (Architectural Component & Module Split)**

应用将遵循模块化和分层架构，主要分为四个核心层：

* **视图层 (View Layer):** React Components, Page Modules, Component Library。
* **状态/业务逻辑层:** **Zustand Stores** (Auth Store, Progress Store, Review Store)。
* **数据服务层 (Data Services Layer):**
    * **Local DB Service:** 使用 **PouchDB/Dexie.js** 提供本地数据的 CRUD 接口。
    * **Sync Service:** 负责处理本地数据与远程服务器数据的**双向同步逻辑**。
* **PWA 核心层 (PWA Core Layer):** Service Worker API，负责缓存和**网络状态检测**。

### **3. 核心功能实现细节 (Core Feature Implementation Details)**

* **离线与同步机制:** 用户操作直接读写 **PouchDB**；网络恢复时，**`Sync Service`** 自动将本地数据推送到远程 API。
* **自定义内容持久化:** 用户创建的自定义卡片将直接存储到 **PouchDB** 的 `custom-cards` 集合中。
* **高效复习与 SRS 算法:** **`Review Store`** 从 **PouchDB** 加载卡片，并使用 **SRS 算法** (如 SuperMemo-2 变种) 来计算复习内容的优先级。