---
title: Functions の Azure Cosmos DB バインド 2.x (プレビュー)
description: Azure Functions で Azure Cosmos DB のトリガーとバインドを使用する方法について説明します。
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure Functions, 関数, イベント処理, 動的コンピューティング, サーバーなしのアーキテクチャ
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: tdykstra
ms.openlocfilehash: e40ba6bcfa1b6247f62849626d4a7803a76362a1
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234871"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x-preview"></a>Azure Functions の Azure Cosmos DB バインド 2.x (プレビュー)

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [バージョン 1 - 一般公開](functions-bindings-cosmosdb.md)
> * [バージョン 2 - プレビュー](functions-bindings-cosmosdb-v2.md)

この記事では、Azure Functions 2.x で [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) のバインドを操作する方法について説明します。 Azure Functions は、Azure Cosmos DB のトリガー、入力、出力のバインドをサポートしています。

> [!NOTE]
> この記事は、プレビューにある [Azure Functions バージョン 2.x](functions-versions.md) を対象としています。  Functions 1.x でこれらのバインドを使用する方法については、[Azure Functions 1.x の Azure Cosmos DB のバインド](functions-bindings-cosmosdb.md)に関するページを参照してください。
>
> このバインドの元の名称は DocumentDB でした。 Functions バージョン 2.x では、トリガー、バインド、およびパッケージの名称はすべて、Cosmos DB になっています。

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>パッケージ - Functions 2.x

Functions バージョン 2.x の Azure Cosmos DB バインドは、[Microsoft.Azure.WebJobs.Extensions.CosmosDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) NuGet パッケージのバージョン 3.x で提供されています。 バインドのソース コードは、[azure-webjobs-sdk-extensions](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/) GitHub リポジトリにあります。

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>トリガー

Azure Cosmos DB のトリガーは [Azure Cosmos DB 変更フィード](../cosmos-db/change-feed.md)を使用して、パーティション間の挿入と更新をリッスンします。 変更フィードは、削除ではなく挿入と更新を発行します。

## <a name="trigger---example"></a>トリガー - 例

言語固有の例をご覧ください。

* [C#](#trigger---c-example)
* [C# スクリプト (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[トリガーの例をスキップする](#trigger---attributes)

### <a name="trigger---c-example"></a>トリガー - C# の例

次の例では、指定されたデータベースとコレクション内に挿入または更新がある場合に呼び出される [C# 関数](functions-dotnet-class-library.md)を示します。

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

[トリガーの例をスキップする](#trigger---attributes)

### <a name="trigger---c-script-example"></a>トリガー - C# スクリプトの例

次の例は、*function.json* ファイルの Cosmos DB トリガー バインドと、そのバインドが使用される [C# スクリプト関数](functions-reference-csharp.md)を示しています。 この関数は、Cosmos DB レコードが変更されるとログ メッセージを書き込みます。

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

C# スクリプト コードを次に示します。
 
```cs 
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
      log.Verbose("Documents modified " + documents.Count);
      log.Verbose("First document Id " + documents[0].Id);
    }
```

[トリガーの例をスキップする](#trigger---attributes)

### <a name="trigger---javascript-example"></a>トリガー - JavaScript の例

次の例は、*function.json* ファイルの Cosmos DB トリガー バインドと、そのバインドが使用される [JavaScript 関数](functions-reference-node.md)を示しています。 この関数は、Cosmos DB レコードが変更されるとログ メッセージを書き込みます。

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "type": "cosmosDBTrigger",
    "name": "documents",
    "direction": "in",
    "leaseCollectionName": "leases",
    "connectionStringSetting": "<connection-app-setting>",
    "databaseName": "Tasks",
    "collectionName": "Items",
    "createLeaseCollectionIfNotExists": true
}
```

JavaScript コードを次に示します。

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

## <a name="trigger---attributes"></a>トリガー - 属性

[C# クラス ライブラリ](functions-dotnet-class-library.md)では、[CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) 属性を使用します。

この属性のコンストラクターにはデータベース名とコレクションを指定します。 これらの設定や構成できる他のプロパティについて詳しくは、「[トリガー - 構成](#trigger---configuration)」をご覧ください。 メソッド シグネチャでの `CosmosDBTrigger` 属性の例を次に示します。

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

完全な例については、「[トリガー - C# の例](#trigger---c-example)」を参照してください。

## <a name="trigger---configuration"></a>トリガー - 構成

次の表は、*function.json* ファイルと `CosmosDBTrigger` 属性で設定したバインド構成のプロパティを説明しています。

|function.json のプロパティ | 属性のプロパティ |説明|
|---------|---------|----------------------|
|**type** || `cosmosDBTrigger` に設定する必要があります。 |
|**direction** || `in` に設定する必要があります。 このパラメーターは、Azure Portal でトリガーを作成するときに自動で設定されます。 |
|**name** || 変更されるドキュメントの一覧を表す、関数コードで使用する変数の名前。 | 
|**connectionStringSetting**|**ConnectionStringSetting** | 監視されている Azure Cosmos DB アカウントに接続するために使用される接続文字列を含めたアプリ設定の名前。 |
|**databaseName**|**DatabaseName**  | 監視されているコレクションを使用する Azure Cosmos DB データベースの名前。 |
|**collectionName** |**CollectionName** | 監視されているコレクションの名前。 |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (省略可能) リース コレクションを保持するサービスへの接続文字列を含むアプリ設定の名前。 この値を設定しない場合、`connectionStringSetting` という値が使用されます。 このパラメーターは、ポータルでバインドが作成されるときに自動で設定されます。 リース コレクションの接続文字列には書き込み権限が必要です。|
|**leaseDatabaseName** |**LeaseDatabaseName** | (省略可能) リースの保存のために使用するコレクションを保持しているデータベースの名前。 この値を設定しない場合、`databaseName` の設定の値が使用されます。 このパラメーターは、ポータルでバインドが作成されるときに自動で設定されます。 |
|**leaseCollectionName** | **LeaseCollectionName** | (省略可能) リースの保存のために使用するコレクションの名前。 この値を設定しない場合、`leases` という値が使用されます。 |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (省略可能) `true` を設定すると、リース コレクションが存在していない場合に自動でリース コレクションを作成します。 既定値は `false` です。 |
|**leasesCollectionThroughput**| **leasesCollectionThroughput**| (省略可能) リース コレクションの作成に割り当てる要求ユニットの量を定義します。 この設定は、`createLeaseCollectionIfNotExists` が `true` に設定されている場合のみ使用できます。 このパラメーターは、ポータルでバインドが作成されるときに自動で設定されます。
|**leaseCollectionPrefix**| **LeaseCollectionPrefix**| (省略可能) 設定すると、この関数のリース コレクション内に作成されたリースにプレフィックスが追加され、2 つの独立した Azure Functions が、異なるプレフィックスを使用することで同じリース コレクションを効率的に共有できます。
|**feedPollDelay**| **FeedPollDelay**| (省略可能) 設定すると、現在の変更がすべて排除された後、パーティションでフィードの新しい変更をポーリングするまでの遅延時間がミリ秒単位で定義されます。 既定値は 5,000 (5 秒) です。
|**leaseAcquireInterval**| **LeaseAcquireInterval**| (省略可能) 設定すると、パーティションが既知のホスト インスタンス間で均等に分散されているかどうかを計算するタスクを開始する間隔がミリ秒単位で定義されます。 既定値は 13,000 (13 秒) です。
|**leaseExpirationInterval**| **LeaseExpirationInterval**| (省略可能) 設定すると、パーティションを表すリースでリースを取得する間隔がミリ秒単位で定義されます。 この間隔内にリースが更新されない場合、リースは期限切れとなり、パーティションの所有権は別のインスタンスに移動します。 既定値は 60,000 (60 秒) です。
|**leaseRenewInterval**| **LeaseRenewInterval**| (省略可能) 設定すると、インスタンスが現在保持しているパーティションのすべてのリースの更新間隔がミリ秒単位で定義されます。 既定値は 17,000 (17 秒) です。
|**checkpointFrequency**| **CheckpointFrequency**| (省略可能) 設定すると、リース チェックポイントの間隔がミリ秒単位で定義されます。 既定値は、常に成功した関数呼び出しの後になります。
|**maxItemsPerInvocation**| **MaxItemsPerInvocation**| (省略可能) 設定すると、関数呼び出しごとに受信するアイテムの最大量がカスタマイズされます。

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>トリガー - 使用方法

トリガーは、パーティションに_リース_を保存するために使用する 2 つ目のコレクションを必要とします。 トリガーが機能するためには、監視されるコレクションと、リースを含むコレクションの両方が使用できる必要があります。

>[!IMPORTANT]
> 複数の関数が同じコレクションの Cosmos DB トリガーを使用するように構成している場合、各関数では専用のリース コレクションを使用するか、または各関数に対して異なる `LeaseCollectionPrefix`を指定する必要があります。 専用のリース コレクションを使用していない場合は、関数のうちの 1 つしかトリガーされません。 プレフィックスの詳細については、「[構成](#trigger---configuration)」セクションを参照してください。

トリガーは、ドキュメントが更新または挿入されたかどうかを示すわけではなく、単にドキュメント自体を提供します。 更新と挿入を別の方法で処理する必要がある場合は、挿入または更新のタイムスタンプを実装することでこれを実現できます。

## <a name="input"></a>入力

Azure Cosmos DB 入力バインドでは、1 つ以上の Azure Cosmos DB ドキュメントを取得して関数の入力パラメーターに渡します。 ドキュメント ID またはクエリ パラメーターは、関数を呼び出したトリガーに基づいて決定することができます。 

>[!NOTE]
> Cosmos DB アカウントで MongoDB API を使用している場合は、Azure Cosmos DB の入力または出力バインディングを使用しないでください。 データの破損を引き起こす可能性があります。

## <a name="input---examples"></a>入力 - 例

次のリンクから、ID 値を指定することで単一のドキュメントを読み取る言語固有の例を参照してください。

* [C#](#input---c-examples)
* [C# スクリプト (.csx)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[入力の例をスキップする](#input---attributes)

### <a name="input---c-examples"></a>入力 - C# の例

このセクションには、次の例が含まれています。

* [キュー トリガー、JSON からの ID の検索](#queue-trigger-look-up-id-from-json-c)
* [HTTP トリガー、クエリ文字列からの ID の検索](#http-trigger-look-up-id-from-query-string-c)
* [HTTP トリガー、ルート データからの ID の検索](#http-trigger-look-up-id-from-route-data-c)
* [HTTP トリガー、SqlQuery を使用したルート データからの ID の検索](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP トリガー、SqlQuery を使用した複数のドキュメントの取得](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP トリガー、DocumentClient を使用した複数のドキュメントの取得](#http-trigger-get-multiple-docs-using-documentclient-c)

例では、次のようなシンプルな `ToDoItem` タイプを参照します。

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[入力の例をスキップする](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>キュー トリガー、JSON からの ID の検索 (C#)

次の例は、単一のドキュメントを取得する [C# 関数](functions-dotnet-class-library.md)を示しています。 関数は、JSON オブジェクトを含むキュー メッセージによってトリガーされます。 キュー トリガーは、検索する ID を含む `ToDoItemLookup` という名前のオブジェクトへの JSON を解析します。 該当の ID は、指定されたデータベースとコレクションからの `ToDoItem` ドキュメントの取得に使用されます。

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[入力の例をスキップする](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>HTTP トリガー、クエリ文字列からの ID の検索 (C#)

次の例は、単一のドキュメントを取得する [C# 関数](functions-dotnet-class-library.md)を示しています。 関数は、クエリ文字列を使用して検索のための ID を指定する HTTP 要求によってトリガーされます。 該当の ID は、指定されたデータベースとコレクションからの `ToDoItem` ドキュメントの取得に使用されます。

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[入力の例をスキップする](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>HTTP トリガー、ルート データからの ID の検索 (C#)

次の例は、単一のドキュメントを取得する [C# 関数](functions-dotnet-class-library.md)を示しています。 関数は、ルート データを使用して検索のための ID を指定する HTTP 要求によってトリガーされます。 該当の ID は、指定されたデータベースとコレクションからの `ToDoItem` ドキュメントの取得に使用されます。

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[入力の例をスキップする](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>HTTP トリガー、SqlQuery を使用したルート データからの ID の検索 (C#)

次の例は、単一のドキュメントを取得する [C# 関数](functions-dotnet-class-library.md)を示しています。 関数は、ルート データを使用して検索のための ID を指定する HTTP 要求によってトリガーされます。 該当の ID は、指定されたデータベースとコレクションからの `ToDoItem` ドキュメントの取得に使用されます。 

例では、`SqlQuery` パラメーターでバインド式を使用する方法を示しています。 ここに示したようにルート データを `SqlQuery` パラメーターに渡すことはできますが、現在、[クエリ文字列の値を渡すことはできません](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583)。


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items", 
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[入力の例をスキップする](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP トリガー、SqlQuery を使用した複数のドキュメントの取得 (C#)

次の例は、ドキュメントの一覧を取得する [C# 関数](functions-dotnet-class-library.md)を示しています。 関数は、HTTP 要求によってトリガーされます。 クエリは、`SqlQuery` 属性プロパティで指定されます。

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[入力の例をスキップする](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP トリガー、DocumentClient を使用した複数のドキュメントの取得 (C#)

次の例は、ドキュメントの一覧を取得する [C# 関数](functions-dotnet-class-library.md)を示しています。 関数は、HTTP 要求によってトリガーされます。 コードでは、Azure Cosmos DB バインドによって提供される `DocumentClient` インスタンスを使用して、ドキュメントの一覧を読み取ります。 また、`DocumentClient` インスタンスは、書き込み操作に使用できます。

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.Info($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

[入力の例をスキップする](#input---attributes)

### <a name="input---c-script-examples"></a>入力 - C# スクリプトの例

このセクションには、次の例が含まれています。各例では、さまざまなソースから ID 値を指定して単一のドキュメントを読み取ります。

* キュー トリガー、キュー メッセージからの ID の検索
* キュー トリガー、SqlQuery を使用したキュー メッセージからの ID の検索

[入力の例をスキップする](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-c-script"></a>キュー トリガー、キュー メッセージからの ID の検索 (C# スクリプト)

次の例は、*function.json* ファイルの Cosmos DB 入力バインドと、そのバインドが使用される [C# スクリプト関数](functions-reference-csharp.md)を示しています。 この関数は、1 つのドキュメントを読み取って、そのドキュメントのテキスト値を更新します。

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
これらのプロパティについては、「[構成](#input---configuration)」セクションを参照してください。

C# スクリプト コードを次に示します。

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding 
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

[入力の例をスキップする](#input---attributes)

#### <a name="queue-trigger-look-up-id-queue-message-using-sqlquery-c-script"></a>キュー トリガー、SqlQuery を使用した ID キュー メッセージの検索 (C# スクリプト)

次の例は、*function.json* ファイルの Azure Cosmos DB 入力バインドと、そのバインドが使用される [C# スクリプト関数](functions-reference-csharp.md)を示しています。 この関数は、SQL クエリで指定されている複数のドキュメントを、キュー トリガーを使用してクエリ パラメーターをカスタマイズすることで取得します。

キュー トリガーがパラメーター `departmentId` を提供します。 `{ "departmentId" : "Finance" }` のキュー メッセージは、金融部門のすべてのレコードを返します。 

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

これらのプロパティについては、「[構成](#input---configuration)」セクションを参照してください。

C# スクリプト コードを次に示します。

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {   
        foreach (var doc in documents)
        {
            // operate on each document
        }    
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

[入力の例をスキップする](#input---attributes)

### <a name="input---javascript-examples"></a>入力 - JavaScript の例

このセクションには、次の例が含まれています。各例では、さまざまなソースから ID 値を指定して単一のドキュメントを読み取ります。

* キュー トリガー、キュー メッセージからの ID の検索
* キュー トリガー、SqlQuery を使用したキュー メッセージからの ID の検索

[入力の例をスキップする](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-javascript"></a>キュー トリガー、キュー メッセージからの ID の検索 (JavaScript)

次の例は、*function.json* ファイルの Cosmos DB 入力バインドと、そのバインドが使用される [JavaScript 関数](functions-reference-node.md)を示しています。 この関数は、1 つのドキュメントを読み取って、そのドキュメントのテキスト値を更新します。

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
これらのプロパティについては、「[構成](#input---configuration)」セクションを参照してください。

JavaScript コードを次に示します。

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

[入力の例をスキップする](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-using-sqlquery-javascript"></a>キュー トリガー、SqlQuery を使用したキュー メッセージからの ID の検索 (JavaScript)

次の例は、*function.json* ファイルの Azure Cosmos DB 入力バインドと、そのバインドが使用される [JavaScript 関数](functions-reference-node.md)を示しています。 この関数は、SQL クエリで指定されている複数のドキュメントを、キュー トリガーを使用してクエリ パラメーターをカスタマイズすることで取得します。

キュー トリガーがパラメーター `departmentId` を提供します。 `{ "departmentId" : "Finance" }` のキュー メッセージは、金融部門のすべてのレコードを返します。 

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

これらのプロパティについては、「[構成](#input---configuration)」セクションを参照してください。

JavaScript コードを次に示します。

```javascript
    module.exports = function (context, input) {    
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }       
        context.done();
    };
```

[入力の例をスキップする](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>入力 - F# の例

次の例は、*function.json* ファイルの Cosmos DB 入力バインドと、そのバインドが使用される [F# スクリプト関数](functions-reference-fsharp.md)を示しています。 この関数は、1 つのドキュメントを読み取って、そのドキュメントのテキスト値を更新します。

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```

これらのプロパティについては、「[構成](#input---configuration)」セクションを参照してください。

F# コードを次に示します。

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

この例には、`FSharp.Interop.Dynamic` と `Dynamitey` の NuGet 依存関係を指定する `project.json` ファイルが必要です。

```json
{
    "frameworks": {
        "net46": {
            "dependencies": {
                "Dynamitey": "1.0.2",
                "FSharp.Interop.Dynamic": "3.0.0"
            }
        }
    }
}
```

`project.json` ファイルの追加方法については、[のパッケージ管理](functions-reference-fsharp.md#package)に関するセクションを参照してください。

## <a name="input---attributes"></a>入力 - 属性

[C# クラス ライブラリ](functions-dotnet-class-library.md)では、[CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) 属性を使用します。

この属性のコンストラクターにはデータベース名とコレクションを指定します。 これらの設定や構成できる他のプロパティについて詳しくは、[この後の構成に関するセクション](#input---configuration)をご覧ください。 

## <a name="input---configuration"></a>入力 - 構成

次の表は、*function.json* ファイルと `CosmosDB` 属性で設定したバインド構成のプロパティを説明しています。

|function.json のプロパティ | 属性のプロパティ |説明|
|---------|---------|----------------------|
|**type**     || `cosmosDB` に設定する必要があります。        |
|**direction**     || `in` に設定する必要があります。         |
|**name**     || 関数のドキュメントを表すバインド パラメーターの名前。  |
|**databaseName** |**DatabaseName** |ドキュメントを含むデータベース。        |
|**collectionName** |**CollectionName** | ドキュメントを含むコレクションの名前。 |
|**id**    | **Id** | 取得するドキュメントの ID。 このプロパティは、[バインド式](functions-triggers-bindings.md#binding-expressions-and-patterns)をサポートしています。 **id** プロパティと **sqlQuery** プロパティの両方は設定しないでください。 いずれも設定しない場合は、コレクション全体が取得されます。 |
|**sqlQuery**  |**SqlQuery**  | 複数のドキュメントを取得するときに使用する Azure Cosmos DB SQL クエリ。 このプロパティは、次の例のように実行時のバインドをサポートします。`SELECT * FROM c where c.departmentId = {departmentId}` **id** プロパティと **sqlQuery** プロパティの両方は設定しないでください。 いずれも設定しない場合は、コレクション全体が取得されます。|
|**connectionStringSetting**     |**ConnectionStringSetting**|Azure Cosmos DB 接続文字列を含むアプリ設定の名前。        |
|**partitionKey**|**PartitionKey**|参照用のパーティション キー値を指定します。 バインディング パラメーターを含めることもできます。|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>入力 - 使用方法

C# および F# 関数では、関数が正常に終了したときに、名前付き入力パラメーターを介した入力ドキュメントへの変更がすべて自動的に保持されます。 

JavaScript 関数の場合、関数の終了時に更新が自動的に行われることはありません。 代わりに、`context.bindings.<documentName>In` と `context.bindings.<documentName>Out` を使用して、更新を行います。 [JavaScript の例](#input---javascript-example)を参照してください。

## <a name="output"></a>出力

Azure Cosmos DB 出力バインドを使用すると、Azure Cosmos DB データベースに新しいドキュメントを記述できます。 

>[!NOTE]
> Cosmos DB アカウントで MongoDB API を使用している場合は、Azure Cosmos DB の入力または出力バインディングを使用しないでください。 データの破損を引き起こす可能性があります。

## <a name="output---example"></a>出力 - 例

言語固有の例をご覧ください。

* [C#](#output---c-examples)
* [C# スクリプト (.csx)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

`DocumentClient` を使用した[入力の例](#input---c-examples)も参照してください。

[出力の例をスキップする](#output---attributes)

### <a name="ouput---c-examples"></a>出力 - C# の例

このセクションには、次の例が含まれています。

* キュー トリガー、1 つのドキュメントの書き込み
* キュー トリガー、IAsyncCollector を使用したドキュメントの書き込み

例では、次のようなシンプルな `ToDoItem` タイプを参照します。

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[出力の例をスキップする](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>キュー トリガー、1 つのドキュメントの書き込み (C#)

次の例は、キュー ストレージからのメッセージで提供されるデータを使用して、ドキュメントをデータベースに追加する[ C# 関数](functions-dotnet-class-library.md)を示しています。

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

[出力の例をスキップする](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>キュー トリガー、IAsyncCollector を使用したドキュメントの書き込み (C#)

次の例は、キュー メッセージ JSON で提供されるデータを使用して、ドキュメントのコレクションをデータベースに追加する[ C# 関数](functions-dotnet-class-library.md)を示しています。

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[出力の例をスキップする](#output---attributes)

### <a name="output---c-script-examples"></a>出力 - C# スクリプトの例

次の例は、*function.json* ファイルの Azure Cosmos DB 出力バインドと、そのバインドが使用される [C# スクリプト関数](functions-reference-csharp.md)を示しています。 この関数は、次の形式で JSON を受信するキューのキュー入力バインドを使用します。

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

この関数は、各レコードに対して次の形式の Azure Cosmos DB ドキュメントを作成します。

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

これらのプロパティについては、「[構成](#output---configuration)」セクションを参照してください。

C# スクリプト コードを次に示します。

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
    using Newtonsoft.Json.Linq;

    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
      log.Info($"C# Queue trigger function processed: {myQueueItem}");

      dynamic employee = JObject.Parse(myQueueItem);

      employeeDocument = new {
        id = employee.name + "-" + employee.employeeId,
        name = employee.name,
        employeeId = employee.employeeId,
        address = employee.address
      };
    }
```

複数のドキュメントを作成するために `ICollector<T>` または `IAsyncCollector<T>` にバインドできます。`T` は、サポートされている型のいずれかです。

[出力の例をスキップする](#output---attributes)

### <a name="output---javascript-examples"></a>出力 - JavaScript の例

次の例は、*function.json* ファイルの Azure Cosmos DB 出力バインドと、そのバインドが使用される [JavaScript 関数](functions-reference-node.md)を示しています。 この関数は、次の形式で JSON を受信するキューのキュー入力バインドを使用します。

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

この関数は、各レコードに対して次の形式の Azure Cosmos DB ドキュメントを作成します。

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```

これらのプロパティについては、「[構成](#output---configuration)」セクションを参照してください。

JavaScript コードを次に示します。

```javascript
    module.exports = function (context) {

      context.bindings.employeeDocument = JSON.stringify({ 
        id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
        name: context.bindings.myQueueItem.name,
        employeeId: context.bindings.myQueueItem.employeeId,
        address: context.bindings.myQueueItem.address
      });

      context.done();
    };
```

[出力の例をスキップする](#output---attributes)

### <a name="output---f-examples"></a>出力 - F# の例

次の例は、*function.json* ファイルの Azure Cosmos DB 出力バインドと、そのバインドが使用される [F# 関数](functions-reference-fsharp.md)を示しています。 この関数は、次の形式で JSON を受信するキューのキュー入力バインドを使用します。

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

この関数は、各レコードに対して次の形式の Azure Cosmos DB ドキュメントを作成します。

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

*function.json* ファイルのバインディング データを次に示します。

```json
{
    "name": "employeeDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "out"
}
```
これらのプロパティについては、「[構成](#output---configuration)」セクションを参照してください。

F# コードを次に示します。

```fsharp
    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
      id: string
      name: string
      employeeId: string
      address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
      log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
      let employee = JObject.Parse(myQueueItem)
      employeeDocument <-
        { id = sprintf "%s-%s" employee?name employee?employeeId
          name = employee?name
          employeeId = employee?employeeId
          address = employee?address }
```

この例には、`FSharp.Interop.Dynamic` と `Dynamitey` の NuGet 依存関係を指定する `project.json` ファイルが必要です。

```json
{
    "frameworks": {
        "net46": {
          "dependencies": {
            "Dynamitey": "1.0.2",
            "FSharp.Interop.Dynamic": "3.0.0"
           }
        }
    }
}
```

`project.json` ファイルの追加方法については、[のパッケージ管理](functions-reference-fsharp.md#package)に関するセクションを参照してください。

## <a name="output---attributes"></a>出力 - 属性

[C# クラス ライブラリ](functions-dotnet-class-library.md)では、[CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) 属性を使用します。

この属性のコンストラクターにはデータベース名とコレクションを指定します。 これらの設定や構成できる他のプロパティについて詳しくは、「[出力 - 構成](#output---configuration)」をご覧ください。 メソッド シグネチャでの `CosmosDB` 属性の例を次に示します。

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

完全な例については、「[出力 - C# の例](#output---c-example)」を参照してください。

## <a name="output---configuration"></a>出力 - 構成

次の表は、*function.json* ファイルと `CosmosDB` 属性で設定したバインド構成のプロパティを説明しています。

|function.json のプロパティ | 属性のプロパティ |説明|
|---------|---------|----------------------|
|**type**     || `cosmosDB` に設定する必要があります。        |
|**direction**     || `out` に設定する必要があります。         |
|**name**     || 関数のドキュメントを表すバインド パラメーターの名前。  |
|**databaseName** | **DatabaseName**|ドキュメントが作成されたコレクションを含むデータベース。     |
|**collectionName** |**CollectionName**  | ドキュメントが作成されたコレクションの名前。 |
|**createIfNotExists**  |**CreateIfNotExists**    | コレクションが存在しないときに作成するかどうかを示すブール値。 既定値は *false* です。新しいコレクションは予約済みのスループットで作成され、これが価格に影響を及ぼすためです。 詳細については、 [価格に関するページ](https://azure.microsoft.com/pricing/details/cosmos-db/)を参照してください。  |
|**partitionKey**|**PartitionKey** |`CreateIfNotExists` が true の場合に、作成されるコレクションのパーティション キー パスを定義します。|
|**collectionThroughput**|**CollectionThroughput**| `CreateIfNotExists` が true の場合に、作成されるコレクションの[スループット](../cosmos-db/set-throughput.md)を定義します。|
|**connectionStringSetting**    |**ConnectionStringSetting** |Azure Cosmos DB 接続文字列を含むアプリ設定の名前。        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>出力 - 使用方法

既定では、関数の出力パラメーターに書き込むと、ドキュメントがデータベースに作成されます。 このドキュメントには、自動的に生成された GUID がドキュメント ID として割り当てられます。 出力ドキュメントのドキュメント ID を指定するには、出力パラメーターに渡された JSON オブジェクトで `id` プロパティを指定します。 

> [!Note]  
> 既存のドキュメントの ID を指定した場合、既存のドキュメントは新しい出力ドキュメントによって上書きされます。 

## <a name="exceptions-and-return-codes"></a>例外とリターン コード

| バインド | リファレンス |
|---|---|
| Cosmos DB | [CosmosDB のエラー コード](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [Cosmos DB トリガーを使用するクイックスタートに進む](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Cosmos DB のサーバーレス データベース コンピューティングの詳細情報](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Azure Functions のトリガーとバインドの詳細情報](functions-triggers-bindings.md)
