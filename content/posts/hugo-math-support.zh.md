---
title: "Hugo 数学公式支持：KaTeX 集成完全指南"
date: 2025-09-27T19:00:00+08:00
draft: false
tags: ["Hugo", "KaTeX", "数学", "教程"]
categories: ["Hugo折腾笔记"]
summary: "详细介绍如何在 Hugo 中启用 KaTeX 数学公式渲染，包含完整配置代码和测试示例"
math: true
---

## 概述

Hugo 从版本 0.123.0 开始，通过 Goldmark 的 **passthrough 扩展** 官方支持数学公式渲染。这是 Hugo 核心功能，**不依赖特定主题**，可在任何 Hugo 站点中使用。

## 技术背景

- **Hugo 官方支持**：通过 Goldmark 的 passthrough 扩展实现
- **主题无关**：适用于所有 Hugo 主题，不需要主题特定支持
- **渲染引擎**：使用 KaTeX（比 MathJax 更快更轻量）
- **语法兼容**：支持标准 LaTeX 数学语法

## 实现步骤

### 1. 配置 Hugo

在 `config.toml` 中添加以下配置：

```toml
[params]
  math = true  # 全局启用数学支持（可选）

[markup]
  [markup.goldmark]
    [markup.goldmark.extensions]
      [markup.goldmark.extensions.passthrough]
        enable = true
        [markup.goldmark.extensions.passthrough.delimiters]
          block = [["\\[", "\\]"], ["$$", "$$"]]
          inline = [["\\(", "\\)"], ["$", "$"]]
    [markup.goldmark.renderer]
      unsafe = true  # 允许 HTML 标签
```

### 2. 创建头部资源加载文件

创建 `layouts/partials/extend_head.html`：

```html
{{- /* 按需加载 KaTeX CSS */ -}}
{{- if or (.Param "math") (site.Params.math) -}}
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" crossorigin="anonymous">
{{- end -}}
```

### 3. 创建底部脚本加载文件

创建 `layouts/partials/extend_footer.html`：

```html
{{- /* 按需加载 KaTeX JS 和自动渲染脚本 */ -}}
{{- if or (.Param "math") (site.Params.math) -}}
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" crossorigin="anonymous"></script>
  <script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" crossorigin="anonymous"
          onload="renderMathInElement(document.body,{delimiters:[{left:'$$',right:'$$',display:true},{left:'\\[',right:'\\]',display:true},{left:'\\(',right:'\\)',display:false},{left:'$',right:'$',display:false}]});"></script>
{{- end -}}
```

### 4. 文章中启用数学

在文章的 front matter 中添加：

```yaml
---
title: "我的数学文章"
math: true  # 为此页面启用数学支持
---
```

## 语法说明

### 行内公式

使用 `\(` 和 `\)` 或 `$` 包围：

```markdown
这是行内公式：\( E = mc^2 \)
这是行内公式：$ E = mc^2 $
```

渲染效果：这是行内公式：\( E = mc^2 \) 或 这是行内公式：$ E = mc^2 $

### 块级公式

使用 `$$` 或 `\[` `\]` 包围：

```markdown
$$
\int_{-\infty}^{\infty} e^{-x^2} \, dx = \sqrt{\pi}
$$
```

渲染效果：
$$
\int_{-\infty}^{\infty} e^{-x^2} \, dx = \sqrt{\pi}
$$

## 测试示例

### 基础公式

欧拉恒等式：\( e^{i\pi} + 1 = 0 \)

二次公式：\( x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a} \)

### 矩阵

$$
\begin{bmatrix}
1 & 2 & 3 \\
4 & 5 & 6 \\
7 & 8 & 9
\end{bmatrix}
$$

### 求和与积分

$$
\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}
$$

$$
\int_0^1 x^2 \, dx = \frac{1}{3}
$$

### 分数与根式

$$
\frac{d}{dx}\left(\sqrt{x}\right) = \frac{1}{2\sqrt{x}}
$$

### 希腊字母与特殊符号

$$
\alpha, \beta, \gamma, \Delta, \Omega, \infty, \leq, \geq, \neq
$$

### 上下标

$$
x^{2n+1} + y_{i,j} + z_k^{(m)}
$$

### 极限

$$
\lim_{x \to 0} \frac{\sin x}{x} = 1
$$

### 偏导数

$$
\frac{\partial^2 f}{\partial x \partial y} = \frac{\partial^2 f}{\partial y \partial x}
$$

## 优化建议

1. **按需加载**：使用页面级 `math: true` 而非全局启用，减少不必要的资源加载
2. **CDN 选择**：可替换为其他 CDN 或本地托管的 KaTeX 资源
3. **版本管理**：定期更新 KaTeX 版本号以获得最新功能和修复

## 参考资料

- [Hugo Passthrough Extension 官方文档](https://gohugo.io/render-hooks/passthrough/)
- [KaTeX 官方文档](https://katex.org/)
- [LaTeX 数学符号参考](https://katex.org/docs/supported.html)
