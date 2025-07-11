这种设计使得参会者的贡献不仅在整体上直观呈现（以线段平行于主轴的方式），还能在微观上明确地展现每个参会者的具体贡献区域。

以下是具体化设计方案：

---

# 🌟 GraphMeeting参会者参与状态的双模式记录设计

为了最直观、最清晰地呈现参会者的贡献状态，同时采用两种记录方式：

* **宏观方式**：参会者的整体在线状态以**平行于主轴的线段（筒状外环）** 直观显示在线时间区间。
* **微观方式**：将参会者在线时间的重要节点（每次贡献、被AI提取的节点）靠近主轴上的对应关键节点，以便清晰展现贡献区域。

---

## 🎯 一、双模式参会记录具体方案

### 🔖 **宏观在线状态（外环）**

* 以**圆柱状的透明筒环结构**环绕主轴，表示用户整体在线区间。
* 每位参会者拥有自己独立的一层环筒，以不同颜色或头像标识区别。
* 筒上每条**纵向线段**代表一次连续的在线状态，其高度与主轴上的时间戳对应。

### 📌 **微观贡献节点映射（贴近主轴关键节点）**

* 每个参会者贡献的**关键节点**，以**小圆形头像标记**直接映射到主轴附近的图谱结构中。
* 这些标记节点直接对应主轴关键节点，清晰呈现贡献区域。
* 同时参会者自身的时间线（外环）上也记录了这些节点，但从视觉上采用类似\*\*“投影”\*\*的方式，将自身节点向主轴关键节点投射连接，便于直接查看与追踪。

---

## 🗃 二、数据结构设计建议

数据结构可供参考：

### 📍 **1. 宏观参会状态数据结构**

```python
class ParticipantInterval:
    def __init__(self, user_id, start_timestamp, end_timestamp):
        self.user_id = user_id            # 用户唯一标识
        self.start_timestamp = start_timestamp  # 在线开始时间
        self.end_timestamp = end_timestamp      # 在线结束时间
```

用户整体在线状态数据：

```python
class UserParticipation:
    def __init__(self, user_id):
        self.user_id = user_id
        self.intervals = []  # 存储多个ParticipantInterval

    def add_interval(self, start_timestamp, end_timestamp):
        interval = ParticipantInterval(self.user_id, start_timestamp, end_timestamp)
        self.intervals.append(interval)
```

### 🧩 **2. 微观贡献节点数据结构**

微观贡献节点的设计和[@苍穹Crescendo](https://space.bilibili.com/3546656385534288/dynamic)之前提出的节点结构基本相同，增加参会者贡献标识即可：

```python
class ContributionNode:
    def __init__(self, id, content, timestamp, author_id, r, theta, associated_main_node_id):
        self.id = id                              # 节点ID
        self.content = content                    # 原始提交内容（AI提取或关键贡献）
        self.timestamp = timestamp                # 提交时间
        self.author_id = author_id                # 贡献的参会者ID
        self.r = r                                # 参会者节点与主轴的距离（接近主轴以强调贡献）
        self.theta = theta                        # 参会者节点的角度
        self.associated_main_node_id = associated_main_node_id  # 对应主节点ID（便于快速映射查看）
```

---

## 🌐 三、动画与交互呈现机制建议

### 🎨 **1. 宏观参会状态（外环）动画呈现**

* 用户整体在线区间以外环的透明度、亮度或颜色深浅表达活跃程度。
* 可实现鼠标悬停、点击查看详细信息（在线时长、区间）。

### 🔍 **2. 微观贡献节点动画呈现**

* 当用户在关键节点贡献时，其头像标识以动画形式靠近主轴的对应节点，快速投影连接至主节点。
* AI提取重要信息时，可在头像节点附近自动生成一个小气泡显示原始内容。

---

## 🌠 四、技术实现方式推荐

* **三维渲染**：使用`Three.js`、`Babylon.js` 实现圆柱坐标动画呈现。
* **动画库**：使用`GSAP`、`Anime.js`、`Framer Motion`增强视觉交互效果。
* **数据存储**：Neo4j或DGraph图数据库，支持快速查询参会者与节点关系。

---

## 📈 五、用户体验价值分析

* **宏观视图**：

  * 快速了解整体团队的参与与活跃程度。
  * 清晰掌握整体贡献趋势。

* **微观视图**：

  * 明确观察每个人具体贡献的时间和空间位置。
  * 直接展现用户的贡献价值，激励团队成员的积极参与。
  * 直观分析某个重要节点上具体的贡献者信息。

---

## 🧪 六、需要预研究与验证的重要问题

* 双模式记录在大规模（如几十位甚至上百位参会者）时的视觉可读性和性能负载。
* 参会者贡献节点密集时交互的用户体验优化。
* 节点位置的合理算法（如何高效分布参会者节点避免视觉重叠）。

---

