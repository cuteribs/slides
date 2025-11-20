---
marp: true
header: "Moq vs NSubstitute: A .NET Mocking Showdown"
class:
  - lead
  - invert
---
# **Moq vs NSubstitute**

## A Detailed Comparison of .NET Mocking Frameworks

---

<!--- header: 'Introduction: What is Mocking?' -->

## The Core of Unit Testing is **Isolation**

We need to \"fake\" dependencies to:

*   **Isolate** the code under test.
*   **Control** dependency behavior (e.g., return values, exceptions).
*   **Verify** interactions meet expectations.


**Our Contenders: `Moq` & `NSubstitute`**

---

<!--- header: 'Core Design Philosophies' -->

## Core Design Philosophies: Two Schools of Thought

*   ### **Moq**: **\"Record-Replay / Expectation\"** Model

*   ### **NSubstitute**: **\"Substitute / State-Based\"** Model

---

<!--- header: 'Core Design Philosophies' -->

## Philosophy 1: Moq's \"Record-Replay\" Model

*   ▶︎ **Concept**: Like writing a strict script where you define all the rules upfront.
*   ▶︎ **Feels**: Rigid, explicit, and offers high control.
*   ▶︎ **Flow**: `Setup` (define rules) ➔ `Act` (execute test) ➔ `Verify` (check the script).

---

<!--- header: 'Core Design Philosophies' -->

## Philosophy 2: NSubstitute's \"Substitute\" Model

*   ▶︎ **Concept**: Create a \"substitute\" that behaves more like a real object.
*   ▶︎ **Feels**: Natural, fluent, and highly readable.
*   ▶︎ **Flow**: `Arrange` (configure the sub) ➔ `Act` (interact naturally) ➔ `Assert` (check results/calls).

---

<!--- header: 'Syntax Comparison' -->

## Syntax Comparison

Let's compare them across 5 common scenarios.

`public interface ICalculator { ... }`

---

<!--- header: 'Syntax Comparison (1/5): Creating a Substitute' -->

## Syntax Comparison (1/5): Creating a Substitute

### Moq
```csharp
// Two steps: create the Mock, then get the .Object
var mock = new Mock<ICalculator>();
var calculator = mock.Object;
```

### NSubstitute
```csharp
// One step
var calculator = Substitute.For<ICalculator>();
```
**NSubstitute is more concise.**

---

<!--- header: 'Syntax Comparison (2/5): Stubbing Return Values' -->

## Syntax Comparison (2/5): Stubbing Return Values

Make `Add(1, 2)` return `3`.

### Moq
```csharp
mock.Setup(c => c.Add(1, 2)).Returns(3);
```

### NSubstitute
```csharp
calculator.Add(1, 2).Returns(3);
```
**NSubstitute's fluent API is closer to natural language.**

---

<!--- header: 'Syntax Comparison (3/5): Argument Matching' -->

## Syntax Comparison (3/5): Argument Matching

### Moq (Using `It`)
```csharp
mock.Setup(c => c.Add(It.IsAny<int>(), 5))
    .Returns(10);
```

### NSubstitute (Using `Arg`)
```csharp
calculator.Add(Arg.Any<int>(), 5)
          .Returns(10);
```
**Syntax and functionality are very similar.**

---

<!--- header: 'Syntax Comparison (4/5): Verifying Calls' -->

## Syntax Comparison (4/5): Verifying Calls

### Moq
```csharp
// Verify Add(1,2) was called exactly once
mock.Verify(c => c.Add(1, 2), Times.Once());
```

### NSubstitute
```csharp
// Verify Add(1,2) was called (at least once)
calculator.Received().Add(1, 2);
```
**NSubstitute is more fluent. Moq's `Times` is more precise.**

---

<!--- header: 'Syntax Comparison (5/5): Handling Properties' -->

## Syntax Comparison (5/5): Handling Properties

### Moq
```csharp
// Requires extra setup to act like a real property
mock.SetupAllProperties();
calculator.Mode = \"Binary\";
```

### NSubstitute
```csharp
// Behaves like a real property by default
calculator.Mode = \"Binary\";
```
**NSubstitute is the clear winner here!**

---

<!--- header: 'Feature & Difference Summary' -->

## Feature & Difference Summary

| Feature | Moq | NSubstitute |
| :--- | :--- | :--- |
| **API Style** | Lambda-based `Setup`/`Verify` | Fluent, natural language-like |
| **Learning Curve** | Steeper | **Very Gentle** |
| **Strict Mode** | **Supported** (`Strict`) | Not supported (loose by default) |
| **Property Handling**| Cumbersome | **Simple & Natural** |

---

<!--- 
header: 'The Turning Point: Moq\\'s SponsorLink Controversy'
backgroundColor: '#7f2121'
color: '#ffffff'
-->

## The Turning Point: Moq's \"SponsorLink\" Controversy

**What Happened? (Moq v4.20.0, Aug 2023)**

*   Introduced the `SponsorLink` dependency.
*   At **compile time**, it collected and uploaded data (hashed developer emails).
*   This action, done **without explicit user consent**, caused a privacy and security storm.

---

<!--- 
header: 'The Turning Point: Moq\\'s SponsorLink Controversy'
backgroundColor: '#7f2121'
color: '#ffffff'
-->

## The Aftermath

*   **Broken Community Trust**: Developers and companies became highly distrustful.

*   **Version Pinning**: Many projects locked Moq to `4.18.4` or earlier.

*   **Rise of Alternatives**: Adoption of `NSubstitute` and `FakeItEasy` surged.

---

<!--- header: 'Conclusion & Recommendation' -->

## Conclusion & Recommendation (1/2): For New Projects

✅ **Top Choice: NSubstitute**

*   **Easy to Learn & Use**: Elegant API, gentle learning curve.
*   **Highly Readable**: Tests are cleaner and focus on business logic.
*   **Healthy Community**: Good reputation, no controversial history.

---

<!--- header: 'Conclusion & Recommendation' -->

## Conclusion & Recommendation (2/2): For Existing Moq Projects

⚠️ **Action Plan**

1.  **Pin Your Version**: Immediately lock Moq to a safe version like `4.18.4` or earlier.

2.  **Incremental Replacement**: Start using NSubstitute for new tests.

3.  **Evaluate Migration**: For long-term projects, planning a gradual migration is a wise move.

---

<!--
backgroundColor: '#2d3e50'
color: '#ffffff'
-->

# Q & A

Thank you!