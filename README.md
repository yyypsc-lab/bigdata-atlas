# 大数据 · 交互图鉴

一份可离线打开的大数据基础概念可视化手册：**11 个可动手实验台 + 术语速查 + 掌握度自测**。

## 交付物

| 文件 | 说明 |
| --- | --- |
| `../大数据基础概念·交互图鉴.html` | **单文件成品**，双击即可打开，无任何外部依赖（CSS/JS 已内联） |
| `index.html` | 开发版入口（引用 `assets/` 下的 css/js） |
| `assets/css/styles.css` | 设计系统：暗/亮双主题、响应式、各组件样式 |
| `assets/js/core.js` | 状态与进度、导航、术语库、主题、Hero 画布、快捷键 |
| `assets/js/w-*.js` | 11 个实验台，各自独立注册到 `Atlas.register()` |
| `tools/cdp-shot.mjs` | 开发自检：CDP 逐节截图 + 收集控制台错误 + 溢出检测 |
| `tools/cdp-smoke.mjs` | 交互冒烟测试：触发各组件真实事件并读取结果 |

## 11 个实验台

1. 数据量尺（KB → ZB，滑杆 + 量级梯）
2. 5V 雷达（拖顶点 / 拖滑杆，实时匹配场景）
3. 数据管道压力测试（流量滑杆 → 瓶颈定位 + 背压）
4. HDFS 切块实验室（块大小 / 副本数 / 机架感知）
5. WordCount 拆解器（Split → Map → Shuffle → Reduce 分步）
6. 窗口与水位线（滚动 / 滑动 / 会话 + 迟到数据）
7. CAP 三角（拖动 + 模拟网络分区 + 真实系统预设）
8. 行存 vs 列存（选列 → 对比真实读取字节）
9. 数据倾斜（倾斜系数 → 加盐前后对比）
10. 架构选型器（5 个约束 → 推荐栈 + 理由 + 反模式）
11. 掌握度自测（9 题 + 按主题拆分画像）

## 重新打包成单文件

```powershell
# 在 bigdata-atlas 目录下
$html = Get-Content index.html -Raw -Encoding UTF8
$css  = Get-Content assets/css/styles.css -Raw -Encoding UTF8
$html = $html.Replace('<link rel="stylesheet" href="assets/css/styles.css">', "<style>`n$css`n</style>")
foreach ($f in @('core','w-scale','w-fivev','w-pipeline','w-hdfs','w-wordcount','w-windows','w-cap','w-storage','w-skew','w-arch','w-quiz')) {
  $code = Get-Content "assets/js/$f.js" -Raw -Encoding UTF8
  $html = $html.Replace("<script src=`"assets/js/$f.js`"></script>", "<script>`n$code`n</script>")
}
[System.IO.File]::WriteAllText("..\大数据基础概念·交互图鉴.html", $html, [System.Text.UTF8Encoding]::new($false))
```

## 说明

- 无网络请求、无第三方库、无字体下载；图表全部由原生 SVG / Canvas / DOM 手绘。
- 进度与答题记录只写入本机 `localStorage`（key: `bigdata-atlas.v1`）。
- 键盘：`/` 术语速查、`?` 快捷键、`J/K` 上下节、`1–9` 跳转、`Esc` 关闭弹窗。
- 右上角可切换明暗主题。
