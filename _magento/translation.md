# 文档翻译

## 使用搜索端点搜索产品

> 原文链接 https://devdocs.magento.com/guides/v2.3/rest/search-endpoint.html

**Magento** 提供了两个搜索产品的端点：

* **GET V1/search**
* **GET V1/products**

*GET V1/search* 提供了用户和游客在店面上搜索产品的功能。*GET V1/products* 的返回结果与 Admin 后台商品栏数据一致。

下表突出显示了这两个端点之间的差异：

|性质|V1/PRODUCTS|V1/SEARCH|
|:---:|:---:|:---:|
|是否需要授权token|是|否|
|是否可以直接获取产品数据|是|否(搜索引擎充当代理)|
|是否可以指定产品属性|是|否|
|响应中包含产品数据|是|否|
|结果是否按相关性排序|否|是|
|响应中是否包含聚合/存储桶|否|是(用于 **quick_search_container** 和 **catalog_view_container** 搜索)|

当你只能想到一些个性化的搜索词时，*V1/search* 通常更有用，它可以挑选出单个或有限的一组产品，并且结果是根据其适用的相关性预排序的。

在其他所有情况下，例如在构建和筛选产品集合时，尤其是在其他特定的后端数据可用时，例如属性的选项ID，*V1/products* 端点时一个不错的选择。

##### **构建 V1/search 查询**

*V1/search* 是通过目录搜索商品。响应结果来已配置的搜索引擎(**Stores** > **Settings** > **Configuration** > **Catalog** > **Catalog Search** > **Search engine**)，结果与使用常规前端搜索返回的结果相同。

所有查询都是针对 **catalogsearch_fulltext** 索引运行的。

*V1/search* 的基本 **REST** 调用结构如下：

```
GET <host>/rest/<store_code>/V1/search?searchCriteria[requestName]=<container_name>&
searchCriteria[filter_groups][0][filters][0][field]=<filter_name>&
searchCriteria[filter_groups][0][filters][0][value]=<search_value>
```

默认情况下，容器名(**container_name**)称可以是以下值之一：

* **quick_search_container**
* **advanced_search_container**
* **catalog_view_container**

过滤器名称(filter_name)和搜索值(search_value)的可能值对于每个容器都是特定的。

每个可搜索属性都定义了一个搜索 boost 值，默认值为 1。但是，某些预定义的属性具有更高的值。例如，sku 的默认 boost 值为 6，而 name 的默认值为 5。

有关使用 *V1/search* 的搜索功能的更多详细信息，请查看 **Magento/CatalogSearch/etc/search_request.xml**。

##### **Quick searches**

**quick_search_container** 请求搜索结果与客户在店面上的搜索结果相同。

默认情况下，过滤器名称(**filter_name**)可以是以下之一：

* **category_ids**
* **price.from**
* **price.to**
* **search_term**
* **visibility**

Quick searches return aggreggations and buckets.

**示例**

下面例子是执行 *数字手表* 的快速搜索：

```
GET <host>/rest/<store_code>/V1/search?searchCriteria[requestName]=<quick_search_container>&
searchCriteria[filter_groups][0][filters][0][field]=search_term&
searchCriteria[filter_groups][0][filters][0][value]=digital watch
```

##### **Advanced searches**

*advanced_search_container* 执行更复杂的搜索，类似于高级搜索页面上的搜索。

高级搜索的默认筛选器可以使用以下[字段]值：

* **category_ids**
* **price.from**
* **price.to**
* **sku**

