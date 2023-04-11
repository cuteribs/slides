---
marp: true
size: 16:9
theme: default
paginate: true
class: 
#  - lead
  - invert
---

# **DORA Metrics in Practice**

> Make the decision first, then implement it.

---

# **What are DORA Metrics?**

> DORA metrics evaluate the performance of a development team by observing the velocity and stability of their development processes.

## 
![](../assets/dora-metrics.drawio.png)

---

# **Variables in DORA Metrics**

 `Not Straightforward`
 `Many Variables`

 ![bg right 80%](../assets/teams.drawio.png)

- Different service or product strategies.
- Different team sizes.
- Different available resources.
- Different management styles.

---

# **Definition of DORA Metrics**

- Deployment Frequency (DF): The number of deployments in a period.
- Lead Time for Changes (LTFC): The time between the first code commit and a production deployment.
- Change Failure Rate (CFR): The number of failure in production caused by the deployment.
- Mean Time to Recovery (MTTR): The time between discovering product failure and completing the fix.

---

# **Our Definition of the metrics**

`How is a deployment defined?`

- Is it a deployment only to the front-end production environment for the end user?
- Is it a deployment also to the middle-end production environment for the end user?
- Is it a deployment also for resource changes/operations in the production environment?
- Is it also a package/SDK release for developers?

---

# **Our Definition of the metrics**

`How is the first code commit defined?`

- Is it the timestamp when a feature work item being changed from `new` to `active`?
- Is it the timestamp when the code related to a feature is pushed at the first time?
- Is it the timestamp when the code related to a feature is committed at the first time?

---

# **Our Definition of the metrics**

`How is change failure defined?`

- Is it considered a service is completely offline?
- Is it considered a service partially loses functionality?
- Is it considered a service encounters a minor issue that does not affect normal use?

---

# **Consequences of Blindly Pursuing DORA Metrics**

- Emphasizing output over culture and collaboration may lead to team members pressuring each other, resulting in tense or even hostile emotions.
- Time and effort may be wasted on frequent development and updates that are not relevant or important.
- The quality of the code may gradually decrease, leading to technical debt accumulation, decreasing maintainability and lowering overall security.
- Development may shift from being customer/feature-oriented to being more manager/metrics-oriented.

---

# Thank you

<!-- footer: with some assistance from ChatGPT --> 