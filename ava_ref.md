# 无需API Key的学术文献查询资源汇总

## 1. Crossref REST API
- 官方文档：https://www.crossref.org/documentation/retrieve-metadata/rest-api/
- Swagger交互测试：https://api.crossref.org/swagger-ui.html
- 基URL：https://api.crossref.org
- 测试示例（通过DOI查询）：
  https://api.crossref.org/works/10.1038/nature12373
  （返回JSON元数据，如果存在则有详细标题、作者等信息）

## 2. OpenAlex API
- 官方文档：https://docs.openalex.org/
- 快速入门：https://docs.openalex.org/how-to-use-the-api/api-overview
- 基URL：https://api.openalex.org
- 测试示例（通过DOI查询）：
  https://api.openalex.org/works/https://doi.org/10.1038/nature12373
  （或 https://api.openalex.org/works?filter=doi:10.1038/nature12373）

## 3. Semantic Scholar API
- 官方文档：https://api.semanticscholar.org/api-docs/
- 产品页面（含教程）：https://www.semanticscholar.org/product/api
- 基URL：https://api.semanticscholar.org/graph/v1
- 测试示例（通过DOI查询论文详情）：
  https://api.semanticscholar.org/graph/v1/paper/DOI:10.1038/nature12373?fields=title,authors,year,abstract
  （公共访问无需key，但有限速）

## 4. PubMed E-utilities (NCBI Entrez API)
- 官方文档：https://www.ncbi.nlm.nih.gov/books/NBK25501/
- 详细参数说明：https://www.ncbi.nlm.nih.gov/books/NBK25499/
- 快速入门：https://www.ncbi.nlm.nih.gov/books/NBK25500/
- 基URL：https://eutils.ncbi.nlm.nih.gov/entrez/eutils/
- 测试示例（先搜索PMID，再获取详情）：
  搜索：https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&term=10.1038/nature12373[doi]
  获取摘要：https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&id=23414506（用实际PMID）

## 5. DataCite REST API
- 官方文档：https://support.datacite.org/docs/api
- 交互参考：https://support.datacite.org/reference/introduction
- 基URL：https://api.datacite.org
- 测试示例（通过DOI查询）：
  https://api.datacite.org/dois/10.1038/nature12373
  （主要用于数据集DOI，但也支持部分文献）

## 6. doi.org（DOI解析器）
- 官方解析文档：https://www.doi.org/the-identifier/resources/factsheets/doi-resolution-documentation
- REST API（Handle查询）：https://doi.org/api/handles/{doi}
- 最简单使用：直接访问 https://doi.org/{doi}
- 测试示例：
  https://doi.org/10.1038/nature12373
  （如果存在，会重定向到论文页面；否则报错）

## 7. Europe PMC REST API
- 官方文档：https://europepmc.org/RestfulWebService
- 基URL：https://www.ebi.ac.uk/europepmc/webservices/rest
- 测试示例（搜索）：
  https://www.ebi.ac.uk/europepmc/webservices/rest/search?query=DOI:10.1038/nature12373&resultType=core&format=json

## 8. BASE (Bielefeld Academic Search Engine)
- HTTP接口指南：https://api.base-search.net/ （需联系申请IP白名单）
- OAI-PMH接口文档：https://oai.base-search.net/
- 注意：搜索API需IP白名单，适合批量 harvesting。

这些资源均无需API key（或公共部分无需），适合验证文献真实性。优先推荐Crossref、OpenAlex和doi.org，最简单直接。

如需更多示例或特定语言的代码，请随时告诉我！