---
title: "Hugo Mermaid 图表支持：完整实现指南"
date: 2025-09-27T19:10:00+08:00
draft: false
tags: ["Hugo", "Mermaid", "图表", "教程"]
categories: ["Hugo折腾笔记"]
summary: "详细介绍如何在 Hugo 中集成 Mermaid 图表功能，包含完整配置代码和各类图表测试示例"
---

## 概述

Hugo 通过 **render hooks** 机制官方支持 Mermaid 图表渲染。这是 Hugo 核心功能的一部分，**不依赖特定主题**，可在任何 Hugo 站点中实现。

## 技术背景

- **Hugo 官方支持**：通过 Markdown render hooks 实现
- **主题无关**：适用于所有 Hugo 主题，不需要主题特定功能
- **渲染引擎**：使用官方 Mermaid JavaScript 库
- **按需加载**：仅在包含 Mermaid 图表的页面加载相关资源

## 实现步骤

### 1. 创建 Mermaid Render Hook

创建 `layouts/_markup/render-codeblock-mermaid.html`：

```html
<div style="text-align: center;">
  <pre class="mermaid">
    {{ .Inner | htmlEscape | safeHTML }}
  </pre>
</div>
{{ .Page.Store.Set "hasMermaid" true }}
```

### 2. 配置按需脚本加载

在 `layouts/partials/extend_footer.html` 中添加：

```html
{{- /* 按需加载 Mermaid 脚本 */ -}}
{{- if .Store.Get "hasMermaid" -}}
  <script type="module">
    import mermaid from 'https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.esm.min.mjs';
    mermaid.initialize({ startOnLoad: true });
  </script>
{{- end -}}
```

### 3. 使用语法

在 Markdown 文档中使用 ```mermaid 代码块：

````markdown
```mermaid
graph TD
    A[开始] --> B{条件判断}
    B -->|是| C[执行操作]
    B -->|否| D[跳过操作]
    C --> E[结束]
    D --> E
```
````

## 支持的图表类型

### 1. 流程图 (Flowchart)

```mermaid
flowchart TD
    A[开始] --> B{用户已登录?}
    B -->|是| C[显示仪表板]
    B -->|否| D[跳转登录页]
    C --> E[加载用户数据]
    D --> F[用户登录]
    F --> G{登录成功?}
    G -->|是| C
    G -->|否| H[显示错误信息]
    H --> D
    E --> I[渲染页面]
    I --> J[结束]
```

### 2. 时序图 (Sequence Diagram)

```mermaid
sequenceDiagram
    participant 用户
    participant 前端
    participant API
    participant 数据库
    
    用户->>前端: 提交登录表单
    前端->>API: 发送登录请求
    API->>数据库: 验证用户凭据
    数据库-->>API: 返回验证结果
    
    alt 验证成功
        API-->>前端: 返回 JWT Token
        前端-->>用户: 跳转到仪表板
    else 验证失败
        API-->>前端: 返回错误信息
        前端-->>用户: 显示登录失败
    end
```

### 3. 甘特图 (Gantt Chart)

```mermaid
gantt
    title 项目开发计划
    dateFormat  YYYY-MM-DD
    section 需求分析
    需求调研           :done,    des1, 2024-01-01,2024-01-15
    需求文档编写       :done,    des2, after des1, 10d
    section 设计阶段
    UI/UX 设计        :active,  des3, 2024-01-20, 15d
    系统架构设计      :         des4, after des3, 10d
    section 开发阶段
    前端开发          :         dev1, after des4, 30d
    后端开发          :         dev2, after des4, 25d
    section 测试阶段
    单元测试          :         test1, after dev1, 10d
    集成测试          :         test2, after dev2, 15d
```

### 4. 状态图 (State Diagram)

```mermaid
stateDiagram-v2
    [*] --> 未登录
    未登录 --> 登录中 : 提交登录
    登录中 --> 已登录 : 验证成功
    登录中 --> 未登录 : 验证失败
    已登录 --> 未登录 : 注销
    
    state 已登录 {
        [*] --> 空闲
        空闲 --> 工作中 : 开始任务
        工作中 --> 空闲 : 完成任务
        工作中 --> 暂停 : 暂停任务
        暂停 --> 工作中 : 恢复任务
    }
```


### 5. 饼图 (Pie Chart)

```mermaid
pie title 网站访问来源
    "搜索引擎" : 45
    "直接访问" : 25
    "社交媒体" : 15
    "邮件营销" : 10
    "其他" : 5
```


## 高级配置

### 自定义样式

可以通过 CSS 自定义 Mermaid 图表样式：

```css
.mermaid {
    background-color: #f8f9fa;
    border: 1px solid #e9ecef;
    border-radius: 8px;
    padding: 20px;
    margin: 20px 0;
}
```

### 主题配置

修改 Mermaid 初始化参数：

```javascript
mermaid.initialize({ 
    startOnLoad: true,
    theme: 'neutral',  // 可选: default, neutral, dark, forest
    themeVariables: {
        primaryColor: '#ff6b6b',
        primaryTextColor: '#fff',
        primaryBorderColor: '#ff4757',
        lineColor: '#5f27cd'
    }
});
```

## 性能优化

1. **按需加载**：脚本仅在包含 Mermaid 图表的页面加载
2. **CDN 缓存**：使用可靠的 CDN 服务提供商
3. **版本锁定**：指定具体版本号避免意外更新

## 常见问题

### 图表不显示
1. 检查代码块语言标识符是否为 `mermaid`
2. 确认 render hook 文件路径正确
3. 检查浏览器控制台是否有 JavaScript 错误

### 样式问题
1. 确认 CSS 加载顺序
2. 检查主题是否覆盖了 Mermaid 样式
3. 使用浏览器开发者工具调试样式

## 参考资料

- [Hugo Diagrams 官方文档](https://gohugo.io/content-management/diagrams/)
- [Mermaid 官方文档](https://mermaid.js.org/)
- [Mermaid 语法参考](https://mermaid.js.org/syntax/flowchart.html)
