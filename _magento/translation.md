# 文档翻译

## 使用搜索端点搜索产品

> 原文链接 https://devdocs.magento.com/guides/v2.3/rest/search-endpoint.html

Magento提供了两个搜索产品的端点：

* **GET V1/search**
* **GET V1/products**

*GET V1/search* 提供了用户和游客在店面上搜索产品的功能。*GET V1/products* 的返回结果与Admin后台商品栏数据一致。

下表突出显示了这两个端点之间的差异：

|性质|V1/PRODUCTS|V1/SEARCH|
|:---:|:---:|:---:|
|是否需要授权token|是|否|
|是否可以直接获取产品数据|是|否(搜索引擎充当代理)|
|是否可以指定产品属性|是|否|
|响应中包含产品数据|是|否|
|结果是否按相关性排序|否|是|
|响应中是否包含聚合/存储桶|否|是(用于**quick_search_container**和**catalog_view_container**搜索)|

当你只能想到一些个性化的搜索词时，*V1/search*通常更有用，它可以挑选出单个或有限的一组产品，并且结果是根据其适用的相关性预排序的。

在其他所有情况下，例如在构建和筛选产品集合时，尤其是在其他特定的后端数据可用时，例如属性的选项ID，*V1/products*端点时一个不错的选择。

##### **构建V1/search查询**
