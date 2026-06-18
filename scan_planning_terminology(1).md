# TLS 扫描规划术语体系

> 讨论时间：2026-06-16
> 关键词：TSP, Station Planning, Tour Planning, Path Planning, Route Planning

---

## 1. 旅行商问题（TSP）

找一条经过每个点**恰好一次**且总距离最短的巡回路线的顺序问题。

| 变体 | 含义 |
|---|---|
| **TSP（旅行商问题）** | 闭环——从起点出发，每个点经过一次，最后返回起点 |
| **最短哈密顿路径 / Open TSP** | 开路——只求依次经过所有点的最短路径，不返回起点 |

工程上统称 TSP，具体场景用 open/closed 区分。

---

## 2. TLS 扫描规划二层结构

```
Scan Planning（总称）
├── ① Station Placement / Scan Position Planning
│     决定：站在哪扫？
│     输出：站点的数量和位置
│
└── ② Tour Planning / Station Sequencing
      决定：先扫哪个后扫哪个？
      输出：站点的访问顺序（TSP 求解）
```

### 常见混淆点

- **Station Planning** 在 TLS 领域有两种含义，容易混淆：
  - 含义 A：**站点布设**（placement）—— 站在哪
  - 含义 B：**遍历顺序**（sequencing/traversal）—— 先扫哪后扫哪
- 精确沟通建议：用 **Station Placement** 表含义 A，用 **Tour Planning** 表含义 B

### 文献术语对照

| 来源 | 放置问题 | 遍历问题 |
|---|---|---|
| "Optimal Position and Path Planning for Stop-and-Go Laserscanning" (ISPRS 2022) | Position Planning | Path Planning |
| "Efficient Tour Planning for a Measurement Vehicle" (Gehrung et al., TUM) | Next Best View (NBV) | Tour Planning (TSP) |
| "Two-stage terrestrial laser scan planning framework" (Measurement 2024) | Stage I | Stage II |
| 本文推荐 | **Station Placement / Scan Position Planning** | **Tour Planning** |

---

## 3. Path 与 Route 的核心区别

### 本质差异：离散 vs 连续

| 维度 | **Route（路线）** | **Path（路径）** |
|---|---|---|
| 本质 | **离散的、拓扑的** | **连续的、几何的** |
| 描述什么 | 去哪些地方、按什么顺序去 | 两点之间具体怎么走 |
| 数据形式 | 节点序列 [A→B→C→D] | 连续曲线（折线/样条/轨迹点列） |
| 关心什么 | 顺序、连通性、总距离 | 避障、几何可行性、曲率约束 |
| 典型问题 | TSP、VRP（车辆路径问题） | A*、RRT 避障路径搜索 |

### 各领域的实际用法

**运输/物流领域**：
- **Route Planning** = TSP/VRP，决定车辆去哪些客户点、什么顺序
- **Path Planning** = 几乎不用（route 定了之后沿道路走即可）

**机器人领域（三层递进）**：

```
Route Planning       → "去哪"     → 任务/目的地序列（A→B→C）
Path Planning        → "怎么走"   → 几何空间中无碰撞的连续曲线
Trajectory Planning  → "怎么走好" → 路径 + 速度/加速度/时间
```

**TLS 领域**：
- **Tour/Route Planning**  → 站点顺序（TSP 求解）
- **Path Planning**       → 站点之间的实际移动路径（避障/可通行性）
- 由于 TLS 站间移动通常无障碍（平地行走），文献有时混用 Path Planning 涵盖两层意思

### 你的场景映射

```
先做 station placement（站点布设）
  → 再做 tour/route planning（访问顺序）
    → 最后做 inter-station path planning（站间移动路径）
```

---

## 4. 英文术语速查表

| 中文 | 推荐英文 | 不推荐（易混淆） |
|---|---|---|
| 旅行商问题 | Traveling Salesman Problem (TSP) | — |
| 开路 TSP | Open TSP / Shortest Hamiltonian Path | — |
| 站点布设 | Station Placement / Scan Position Planning | Station Planning（歧义） |
| 遍历顺序 | Tour Planning / Station Sequencing | Station Planning（歧义） |
| 路线规划 | Route Planning | Path Planning（在几何层面另有含义） |
| 路径规划 | Path Planning | Route Planning（在拓扑层面另有含义） |
| 轨迹规划 | Trajectory Planning | — |

---

## 5. 参考文献

- ISPRS Annals 2022: "Optimal Position and Path Planning for Stop-and-Go Laserscanning for the Acquisition of 3D Building Models"
- Gehrung et al.: "Efficient Tour Planning for a Measurement Vehicle by Combining Next Best View and Traveling Salesman"
- Measurement 2024: "Two-stage terrestrial laser scan planning framework for geometric measurement of civil infrastructures"
- ISPRS 2021: "Optimal scan planning with enforced network connectivity for the acquisition of three-dimensional indoor models"