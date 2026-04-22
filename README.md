<div align="center">

🌐 **[English](#-llm-vram-calculator)** | **[中文](#-llm-显存占用计算器)**

</div>

---

<div align="center">

# 🧮 LLM VRAM Calculator

**A professional GPU VRAM estimation tool for the Qwen3.5 model family.**

[![Live Demo](https://img.shields.io/badge/🔗-Live%20Demo-blue)](https://sos-zachary.github.io/GPUmemory-calculator-demo/)
[![React](https://img.shields.io/badge/-React-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/-Vite-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)

🚀 [https://sos-zachary.github.io/GPUmemory-calculator-demo/](https://sos-zachary.github.io/GPUmemory-calculator-demo/)

</div>

---

## 📝 Overview

LLM VRAM Calculator is an interactive, single-page web application that estimates GPU VRAM consumption for running **Qwen3.5** LLM models under various configurations. It covers the complete model lineup — from the lightweight **0.8B** to the flagship **397B-A17B** MoE model — and supports multiple quantization methods, inference engines, context lengths, and concurrency levels.

### ✨ Features

| Feature | Description |
|---------|-------------|
| 🎛️ **Interactive Config** | Adjust quantization, engine, context length, and concurrency in real time |
| 📊 **Visual Breakdown** | Stacked bar charts and pie charts show memory distribution |
| 💡 **GPU Recommendations** | Get tailored GPU hardware suggestions based on your config |
| 🌗 **Dark / Light Theme** | Seamless theme switching for comfortable viewing |
| 📱 **Responsive Design** | Optimized for both desktop and mobile devices |

---

## 🧪 Formulas

The calculator uses industry-standard simplified estimation formulas:

### 1. Model Weights

$$
\text{Model Weights} = \frac{\text{totalParams} \times \text{bytesPerParam}}{2^{30}} \text{ GB}
$$

> 💡 For **MoE** models, **all** expert parameters are loaded into VRAM.

### 2. KV Cache

$$
\text{KV Cache} = \frac{2 \times \text{layers} \times \text{numKVHeads} \times \text{headDim} \times \text{contextLength} \times \text{concurrency} \times \text{kvBytesPerParam}}{2^{30}} \text{ GB}
$$

> 💡 Calculated using the model's architecture dimensions (activated parameters).

### 3. Activations

$$
\text{Activations} \approx \frac{\text{concurrency} \times \text{contextLength} \times \text{hiddenSize} \times 18 \times \text{bytesPerParam}}{2^{30}} \text{ GB}
$$

### 4. Engine Overhead

$$
\text{Engine Overhead} = (\text{weights} + \text{KV Cache} + \text{activations}) \times \text{overheadFactor}
$$

### 5. Total VRAM

$$
\text{Total VRAM} = \text{weights} + \text{KV Cache} + \text{activations} + \text{Engine Overhead}
$$

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | [React](https://react.dev/) 19 |
| Language | [TypeScript](https://www.typescriptlang.org/) 5.9 |
| Build Tool | [Vite](https://vitejs.dev/) 7 |
| Styling | [Tailwind CSS](https://tailwindcss.com/) 3 |
| UI Components | [shadcn/ui](https://ui.shadcn.com/) |
| Charts | [Recharts](https://recharts.org/) |
| Icons | [Lucide React](https://lucide.dev/) |

---

## 📦 Getting Started

```bash
# Install dependencies
npm install

# Start development server (http://localhost:3000)
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

---

## 📂 Project Structure

```
src/
├── main.tsx          # Entry point
├── App.tsx           # Main application UI (~750 lines)
├── index.css         # Global styles & theme system
├── components/ui/    # 40+ shadcn/ui components
├── data/modelData.ts # Domain models & VRAM calculation logic
├── hooks/useTheme.tsx# Dark / light theme context
└── lib/utils.ts      # Tailwind class merge utility
```

---

## 📄 License

MIT License

---

<div align="center">

# 🧮 LLM 显存占用计算器

**为 Qwen3.5 全系列模型打造的专业 GPU 显存估算工具。**

🚀 [https://sos-zachary.github.io/GPUmemory-calculator-demo/](https://sos-zachary.github.io/GPUmemory-calculator-demo/)

</div>

---

## 📝 项目简介

LLM 显存占用计算器是一款交互式单页 Web 应用，用于估算在不同配置下运行 **Qwen3.5** 大语言模型所需的 GPU 显存。涵盖完整模型阵容——从轻量级的 **0.8B** 到旗舰 **397B-A17B** MoE 模型——并支持多种量化方法、推理引擎、上下文长度和并发数配置。

### ✨ 功能特性

| 特性 | 说明 |
|------|------|
| 🎛️ **交互式配置** | 实时调整量化方式、推理引擎、上下文长度与并发数 |
| 📊 **可视化拆分** | 堆叠柱状图与饼图直观展示显存占用分布 |
| 💡 **GPU 推荐** | 根据当前配置智能推荐合适的 GPU 硬件 |
| 🌗 **深色 / 浅色主题** | 无缝主题切换，保护视力 |
| 📱 **响应式设计** | 完美适配桌面端与移动端 |

---

## 🧪 计算公式

本工具采用业界通用的简化估算公式：

### 1. 模型权重 (Model Weights)

$$
\text{模型权重} = \frac{\text{总参数量(B)} \times \text{每参数字节数}}{2^{30}} \text{ GB}
$$

> 💡 **MoE 模型**：所有专家参数都会加载到显存中。

### 2. KV Cache

$$
\text{KV Cache} = \frac{2 \times \text{层数} \times \text{KV 头数} \times \text{头维度} \times \text{上下文长度} \times \text{并发数} \times \text{KV 每参数字节数}}{2^{30}} \text{ GB}
$$

> 💡 使用模型的架构维度（对应激活参数量）进行计算。

### 3. 激活值 (Activations)

$$
\text{激活值} \approx \frac{\text{并发数} \times \text{上下文长度} \times \text{隐藏层大小} \times 18 \times \text{每参数字节数}}{2^{30}} \text{ GB}
$$

### 4. 引擎开销 (Engine Overhead)

$$
\text{引擎开销} = (\text{权重} + \text{KV Cache} + \text{激活值}) \times \text{开销系数}
$$

### 5. 总显存占用 (Total VRAM)

$$
\text{总显存} = \text{权重} + \text{KV Cache} + \text{激活值} + \text{引擎开销}
$$

---

## 🛠️ 技术栈

| 层级 | 技术 |
|------|------|
| 框架 | [React](https://react.dev/) 19 |
| 语言 | [TypeScript](https://www.typescriptlang.org/) 5.9 |
| 构建工具 | [Vite](https://vitejs.dev/) 7 |
| 样式 | [Tailwind CSS](https://tailwindcss.com/) 3 |
| UI 组件 | [shadcn/ui](https://ui.shadcn.com/) |
| 图表 | [Recharts](https://recharts.org/) |
| 图标 | [Lucide React](https://lucide.dev/) |

---

## 📦 快速开始

```bash
# 安装依赖
npm install

# 启动开发服务器（http://localhost:3000）
npm run dev

# 构建生产版本
npm run build

# 预览生产构建
npm run preview
```

---

## 📂 项目结构

```
src/
├── main.tsx          # 入口文件
├── App.tsx           # 主应用 UI（约 750 行）
├── index.css         # 全局样式与主题系统
├── components/ui/    # 40+ 个 shadcn/ui 组件
├── data/modelData.ts # 领域模型与显存计算逻辑
├── hooks/useTheme.tsx# 深色 / 浅色主题上下文
└── lib/utils.ts      # Tailwind 类名合并工具
```

---

## 📄 许可证

MIT License
