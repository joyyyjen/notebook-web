---
title: "Web Search"
date: 2018-12-26T11:33:37+08:00
---
## Web Search

|Challenges|Application|
|----------|-----------|
|scalability|parallel indexing & searching (MapReduce)|
|low quality information and spams | Spam detection & Robust ranking|
dynamics of the web | --|
|**Opportunities**| many additional heuristics 啟發 can be leveraged 槓桿原理 to improve search accuracy|
|rich link information, layout, etc | link analysis & multi-feature ranking|


Web  &rightarrow; Crawler  &rightarrow; Indexer <-> Retriever <-> Browser  &leftarrow; User

### Major Crawling Strategies:
1. Breadth-First 
2. Parallel Crawing 
3. Variation: focused crawling
    - Targeting at a subset of pages
    - Typically given a query
4. Incremental / Repeated Crawling 
    - need to minimuizer resource overhead
    - can learn from the past experience (updated daily vs. monthly)
    - Target at: 1) frequently uptated pages 2) Frequently accessed pages


### Link Analysis
Ranking Algorithms for Web Search
    Standard IR aren't sufficient for: different information needs, documents that have additional information, or information quality that varies a lot.
**Major Extensions**: 
    exploiting links to improve scoring
    exploiting clickthroughs for massive implicit feedback

#### Exploiting Inter-Document Links