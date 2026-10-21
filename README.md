# PTA Auto Solver · 中文修复版

原项目：[NeuronCState/PTA-Auto-Solver](https://github.com/NeuronCState/PTA-Auto-Solver)（作者 NeuronCState，MIT）

本仓库只做一件事：**修复编程题样例输出提取为空**的问题。已向上游提交 PR：[#3](https://github.com/NeuronCState/PTA-Auto-Solver/pull/3)

## 修复内容

PTA 页面把样例标题渲染成 `样例 输出：`（中间有空格），原来的 `/样例输出/` 正则匹配不到；纯文本兜底又会把「样例解释 / 代码长度限制」一起抓进来。结果是 `sampleOutput` 经常为空，模型看不到期望输出，只能凭题面猜边界，同一处测试点反复出错。

现在改为：

1. **DOM 优先**：按文档顺序取 `pre / code / .cm-editor` 代码块（排除答题编辑器），前两块即样例输入、样例输出；
2. **文本兜底**：所有段落正则做空白容错（`/样\s*例\s*输\s*出/`）；
3. **清洗**：剥离行号与 `[ in ] / [ out ]`、`输入：/ 输出：` 标签，遇到 `样例解释 / 代码长度限制 / 时间限制 / 内存限制 / 栈限制` 立即截断。

## 安装

1. 装好 Tampermonkey；
2. 打开 [`PTA Auto Solver Pro-1.7.0.user.js`](./PTA%20Auto%20Solver%20Pro-1.7.0.user.js) → 点 Raw → 确认安装；
3. 打开 `pintia.cn` 任意题目页面，右下角出现面板即为生效。

## 测试

```
PASS DOM: 输入 100 200
PASS DOM: 输出 10
PASS 文本: 输入 100 200
PASS 文本: 输出 10
PASS 未混入解释
PASS 多组取前两块
PASS 无样例返回空
```

## 未改动

跳过 / 重做逻辑、提示词、模型配置、图片多模态、语言锁定，全部与原版 1.6.0 一致。

## 许可

跟随上游，MIT。
