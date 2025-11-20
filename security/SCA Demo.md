---
marp: true
title: SCA 15‑Minute Demo
paginate: true
theme: default
backgroundImage: url('dnv-bg.png')
---

# Revisiting SCA (Software Composition Analysis)

Goal: Show why dependency risk matters & how to act fast.

---

## Agenda

1. Problem & Impact  
2. Core Concept: What SCA Solves 
3. Node Demo `lodashCase` 
4. .NET Demo `NewtonsoftCase` 

---

## Problem & Impact

- 70–90% of app code = dependencies.
- Exploit speed: hours after CVE disclosure.
- Silent risks: prototype pollution, malicious publish, license traps.
  
Message: Inventory + continuous scanning are baseline hygiene.

---

## What SCA Does

Detects: Known CVEs, malicious packages, license incompatibilities, abandoned libs.
Outputs: Alerts, upgrade paths, SBOM (dependency bill of materials).
Differs from SAST: Looks at what you import, not logic you write.

---

## Demo: Node `lodashCase`

### A Prototype Pollution vulnerability in Lodash v4.17.11

The issue lies in Lodash's deep merge functions not properly sanitizing the \_\_proto\_\_ or constructor.prototype keys. When merging user-controlled data, these keys can be used to modify the Object prototype.

---

## Demo: .NET `NewtonsoftCase`

### A Remote Code Execution (RCE) vulnerability in Newtonsoft.Json v12.0.2

JSON.NET supports polymorphic deserialization using the $type field, which allows the incoming JSON to specify the concrete type to instantiate. While useful for legitimate polymorphic models, it’s inherently dangerous when exposed to untrusted input.

---

# Q & A

