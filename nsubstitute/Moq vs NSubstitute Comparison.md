---
marp: true
theme: uncover
header: "Moq vs NSubstitute: .NET 模拟框架大对决"
footer: © 2024 - A Detailed Comparison
class:
  - invert
  - lead
---
# **Moq vs NSubstitute**

## .NET 模拟(Mocking)框架的详细对比

---

<!--- header: '背景介绍：什么是模拟 (Mocking)?' -->

## 单元测试的核心是 **隔离**

我们需要“伪造”依赖项，以便：

*   **隔离** 被测代码逻辑
*   **控制** 依赖行为 (如返回值、异常)
*   **验证** 交互是否符合预期


**今天的主角: `Moq` & `NSubstitute`**

---

<!--- header: '核心设计理念' -->

## 核心设计理念：两种流派

*   ### **Moq**: “录制-回放 / 期望” 模式

*   ### **NSubstitute**: “替代品 / 状态” 模式

---

<!--- header: '核心设计理念' -->

## 理念 1: Moq 的 “录制-回放” 模式

*   ▶︎ **理念**: 像编写一个严格的脚本，先定义好所有规则。
*   ▶︎ **感觉**: 严谨、明确、控制力强。
*   ▶︎ **流程**: `Setup` (定义规则) ➔ `Act` (执行测试) ➔ `Verify` (验证脚本)。

---

<!--- header: '核心设计理念' -->

## 理念 2: NSubstitute 的 “替代品” 模式

*   ▶︎ **理念**: 创建一个行为更像真实对象的“替代品”。
*   ▶︎ **感觉**: 自然、流畅、可读性高。
*   ▶︎ **流程**: `Arrange` (配置替身) ➔ `Act` (自然交互) ➔ `Assert` (检查结果/调用)。

---

<!--- header: '语法对比' -->

## 语法对比

接下来，通过 5 个常见场景进行对比。

`public interface ICalculator { ... }`

---

<!--- header: '语法对比 (1/5): 创建替身' -->

## 语法对比 (1/5): 创建替身

### Moq
```csharp
// 两步：创建 Mock，再获取 .Object
var mock = new Mock<ICalculator>();
var calculator = mock.Object;
```

### NSubstitute
```csharp
// 一步到位
var calculator = Substitute.For<ICalculator>();
```
**NSubstitute 更简洁。**

---

<!--- header: '语法对比 (2/5): 设置返回值' -->

## 语法对比 (2/5): 设置返回值

让 `Add(1, 2)` 返回 `3`

### Moq
```csharp
mock.Setup(c => c.Add(1, 2)).Returns(3);
```

### NSubstitute
```csharp
calculator.Add(1, 2).Returns(3);
```
**NSubstitute 的流式 API 更接近自然语言。**

---

<!--- header: '语法对比 (3/5): 参数匹配' -->

## 语法对比 (3/5): 参数匹配

### Moq (使用 `It`)
```csharp
mock.Setup(c => c.Add(It.IsAny<int>(), 5))
    .Returns(10);
```

### NSubstitute (使用 `Arg`)
```csharp
calculator.Add(Arg.Any<int>(), 5)
          .Returns(10);
```
**语法和功能高度相似。**

---

<!--- header: '语法对比 (4/5): 验证调用' -->

## 语法对比 (4/5): 验证调用

### Moq
```csharp
// 验证 Add(1,2) 被调用了一次
mock.Verify(c => c.Add(1, 2), Times.Once());
```

### NSubstitute
```csharp
// 验证 Add(1,2) 被调用了
calculator.Received().Add(1, 2);
```
**NSubstitute 更流畅。Moq 的 `Times` 更精确。**

---

<!--- header: '语法对比 (5/5): 属性处理' -->

## 语法对比 (5/5): 属性处理

### Moq
```csharp
// 需额外设置才能像真实属性一样工作
mock.SetupAllProperties();
calculator.Mode = \"Binary\";
```

### NSubstitute
```csharp
// 默认行为就像真实属性
calculator.Mode = \"Binary\";
```
**NSubstitute 在此场景完胜！**

---

<!--- header: '特性与差异总结' -->

## 特性与差异总结

| 特性 | Moq | NSubstitute |
| :--- | :--- | :--- |
| **API 风格** | 基于 Lambda 的 `Setup`/`Verify` | 流式、接近自然语言 |
| **学习曲线** | 稍陡峭 | **非常平缓** |
| **严格模式** | **支持** (`Strict`) | 不支持 (默认松散) |
| **属性处理** | 繁琐 | **简单自然** |

---

<!--- 
header: '关键转折点: Moq 的 SponsorLink 争议'
backgroundColor: '#7f2121'
color: '#ffffff'
-->

## 关键转折点: Moq 的 SponsorLink 争议

**发生了什么? (Moq v4.20.0, 2023年8月)**

*   引入 `SponsorLink` 依赖。
*   在**编译时**收集并上传数据 (开发者邮箱哈希)。
*   此行为**未经用户明确同意**，引发隐私和安全风暴。

---

<!--- 
header: '关键转折点: Moq 的 SponsorLink 争议'
backgroundColor: '#7f2121'
color: '#ffffff'
-->

## 争议的后果

*   **社区信任破裂**：开发者和企业产生严重不信任感。

*   **版本锁定**：大量项目将 Moq 版本锁定在 `4.18.4` 或更早。

*   **替代品崛起**：`NSubstitute` 和 `FakeItEasy` 的采用率激增。

---

<!--- header: '结论与推荐' -->

## 结论与推荐 (1/2): 新项目

✅ **首选: NSubstitute**

*   **易学易用**: API 优雅简洁，学习曲线平缓。
*   **可读性高**: 测试代码更清晰，更专注于业务逻辑。
*   **社区健康**: 声誉良好，无历史争议。

---

<!--- header: '结论与推荐' -->

## 结论与推荐 (2/2): 现有 Moq 项目

⚠️ **行动指南**

1.  **锁定版本**: 立即将 Moq 版本固定在 `4.18.4` 或更早的安全版本。

2.  **增量替换**: 新的测试代码开始使用 NSubstitute。

3.  **评估迁移**: 对长期维护的项目，制定计划逐步迁移是明智之举。

---

<!--
backgroundColor: '#2d3e50'
color: '#ffffff'
-->

# Q & A

感谢观看！