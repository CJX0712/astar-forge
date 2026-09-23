# AStarForge — A* 寻路锻造炉

<p align="center">
  <a href="https://github.com/CJX0712/astar-forge/actions/workflows/ci.yml"><img src="https://github.com/CJX0712/astar-forge/actions/workflows/ci.yml/badge.svg" alt="ci"></a>
  <a href="https://github.com/CJX0712/astar-forge/releases"><img src="https://img.shields.io/github/v/release/CJX0712/astar-forge?sort=semver" alt="release"></a>
  <a href="https://github.com/CJX0712/astar-forge/blob/main/LICENSE"><img src="https://img.shields.io/github/license/CJX0712/astar-forge" alt="license"></a>
  <img src="https://img.shields.io/badge/author-%E6%99%A8%E6%98%9F-1f6feb" alt="author">
</p>

单文件离线 A* 寻路工具：二叉堆 + Manhattan 启发式，完美迷宫（递归回溯）与随机墙壁两种场景，
动画演示 open/closed 扩张与最终路径，Shift+点击设起点、Alt+点击设终点、普通点击切换墙壁。
零外部依赖，浏览器直接打开即用。

## 引擎（`AStar`，IIFE）

- `Heap` — 二叉最小堆，按 `(f, h)` 字典序比较，附 `valid()` 不变量检查
- `astar(W,H,walls,start,goal)` — A* 主搜索（4 邻接、单位代价、懒删除陈旧节点），返回
  `{found, path, cost, expanded, openPeak}`
- `bfsDist` — BFS 参照距离场（单位代价 Dijkstra），用于最优性交叉验证
- `genMaze(W,H,seed)` — 递归回溯完美迷宫（生成树性质：任意通路单元互相可达）
- `mulberry32` — 确定性 PRNG
- `manhattan` / `neighbors` — 启发式与邻接

## 无头验证（8/8，`node _smoke.js`）

1. 堆不变量：500 随机键 `valid()` + pop 出序单调不降
2. **最优性交叉验证：300 随机网格（20×20，30% 墙）A* 代价 == BFS 距离**，
   同时校验路径合法性（4 邻接、不穿墙、端点正确、`len == cost+1`）
3. 完美迷宫：10 个种子下所有通路单元可达（生成树性质）
4. 确定性：同输入两次运行路径逐位一致
5. 不可达：整行墙 → `found=false`，不崩溃
6. 已知向量：空 10×10 角到角，Manhattan 下代价恰 18、路径长 19
7. 可采纳启发式效率：A* 展开数 ≤ BFS 可达数（50 网格，实测 6688 vs 30214）
8. 退化：起点 == 终点 → 代价 0、路径 `[42]`

## 运行

```bash
node _smoke.js   # 无头验证
node _probe.js   # ASCII 迷宫 + 路径探针
```

或直接用浏览器打开 `index.html`。

## License

MIT
