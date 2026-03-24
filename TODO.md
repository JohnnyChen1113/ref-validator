# Reference Validator - Dev 分支工作交接

## 已完成的改动

### 问题背景
工具在验证 AI 生成的学术引用时，即使年份、DOI、期刊名与数据库返回的结果不一致，仍然显示 "Verified" 且无任何警告。

### 三个测试 Case

1. **年份不匹配**：Kadonaga (2012) → 数据库返回 2011，应警告
2. **DOI 不匹配**：Giardina 引用中 DOI `10.1126/science.8303568` → 实际 `10.1126/science.8342041`
3. **期刊+DOI 都不匹配**：Qiu 引用中 `Genome Research` → 实际 `Genome Biology`，DOI 也是编造的

### 已完成
- [x] i18n 增加 `doiMismatch` / `doiMatch` 中英文翻译（~行 700, 773）
- [x] `parseReference` 增加 `journal` 字段解析，支持 `*斜体*` 和句号后模式（~行 1138）
- [x] `buildMismatchHtml` 移除 `if (result.status !== 'partial') return ''` 限制
- [x] `buildMismatchHtml` 新增 DOI 比较逻辑
- [x] `buildMismatchHtml` 改进 journal 比较，优先使用 `parsed.journal`
- [x] `buildMismatchHtml` 中检测到不匹配时设置 `result.hasWarnings = true`

## 待完成的工作

### 1. renderResult / updateRenderedResult 增加 Warning 样式（优先级高）
**文件**：`index.html`，`renderResult` 函数（~行 1590）和 `updateRenderedResult`（~行 1615）

目前 `result.hasWarnings` 标志已经埋好了，但渲染层还没用上。需要：
- 当 `result.status === 'verified' && result.hasWarnings` 时，在 Verified 标签旁边加一个橙色 ⚠ 警告标识
- 可以考虑新增一个 CSS 样式，比如绿底但带橙色边框，让用户一眼看出"虽然找到了但有字段不对"

参考现有的 `statusConfig` 对象（~行 1592），可以加一个 `verified_warn` 配置：
```js
verified_warn: { 
    bg: 'bg-green-50 dark:bg-green-900/20', 
    border: 'border-yellow-400 dark:border-yellow-600',  // 橙色边框
    icon: '⚠', iconBg: 'bg-yellow-500',  // 警告图标
    label: t.verified, 
    labelClass: '...' 
}
```

然后在选择 config 时判断：
```js
const configKey = (result.status === 'verified' && result.hasWarnings) ? 'verified_warn' : result.status;
const config = statusConfig[configKey];
```

### 2. 验证测试（优先级高）
用以下三条引用测试，确认 mismatch 信息正确显示：

```
Kadonaga, J. T. (2012). Perspectives on the RNA polymerase II core promoter. *Wiley Interdisciplinary Reviews: Developmental Biology*, *1*(1), 40–51. https://doi.org/10.1002/wdev.21

Giardina, C., & Lis, J. T. (1993). DNA melting on yeast RNA polymerase II promoters. *Science*, *261*(5122), 759–762. https://doi.org/10.1126/science.8303568

Qiu, C., Jin, H., Vvedenskaya, I., et al. (2020). Universal promoter scanning by Pol II during transcription initiation in *Saccharomyces cerevisiae*. *Genome Research*, *30*(6), 896–907. https://doi.org/10.1101/gr.257360.119
```

### 3. Table View 也要支持 Warning 显示（优先级中）
`renderTable` 函数（~行 1835）的状态列目前只有三种颜色，需要支持 verified + warning 的情况。

### 4. Copy 功能分类考虑（优先级低）
`copyVerified` 等函数是否需要区分 "verified without warnings" 和 "verified with warnings"？可以考虑加一个 "Copy Warnings" 按钮。

### 5. 边界情况优化（优先级低）
- journal 解析可能在某些非标准格式下提取错误，需要更多测试
- DOI 不存在（AI 编造的 DOI，查询直接 404）的情况，目前会 fallback 到 title search 找到论文，但不会特别标注"原始 DOI 不存在"——目前只会显示 DOI mismatch，足够用了

## 关键代码位置速查

| 内容 | 行号（大约） |
|------|-------------|
| i18n 中文 | 630-702 |
| i18n 英文 | 703-775 |
| parseReference | 976 |
| journal 解析 | 1138-1151 |
| validateMatch（年份惩罚） | 1177 |
| searchCrossref | 1193 |
| searchOpenAlex | 1240 |
| verifyReference（状态判定） | 1403-1432 |
| journalMatches | 1635 |
| buildMismatchHtml | 1662 |
| buildDetailsHtml | 1722 |
| renderResult | ~1590 |
| updateRenderedResult | ~1615 |
| renderTable | ~1835 |
