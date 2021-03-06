---
title: Bing Spell Check API v5 を v7 にアップグレードする | Microsoft Docs
description: バージョン 7 を使用するように更新する必要のあるアプリケーションの部分を識別します。
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7DC8FB29-4732-47D8-824B-CF2D7AEBA07B
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 1b0406641053fac8a4b3f4721728ad3b6c313ba1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2018
ms.locfileid: "35372832"
---
# <a name="spell-check-api-upgrade-guide"></a>Spell Check API のアップグレード ガイド

このアップグレード ガイドでは、Bing Spell Check API のバージョン 5 とバージョン 7 の間の変更を識別します。 このガイドを使用して、バージョン 7 を使用するように更新する必要のあるアプリケーションの部分を識別します。

## <a name="breaking-changes"></a>重大な変更

### <a name="endpoints"></a>エンドポイント

- エンドポイントのバージョン番号は、v5 から v7 に変更されました。 たとえば、https://api.cognitive.microsoft.com/bing/\*\*v7.0**/spellcheck などです。

### <a name="error-response-objects-and-error-codes"></a>エラー応答オブジェクトとエラー コード

- すべての失敗した要求には、応答本文内に`ErrorResponse` オブジェクトが含まれるようになりました。

- `Error` オブジェクトに次のフィールドが追加されました。  
  - `subCode`&mdash;可能であれば、エラー コードを個別のバケットにパーティション分割します
  - `moreDetails`&mdash;`message` フィールドで説明されているエラーに関する追加情報
   

- v5 エラー コードを次の可能な `code` および `subCode` 値に置き換えました。  
  
|コード|SubCode|説明
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|サブコード条件のいずれかが発生するたびに、Bing は ServerError を返します。 HTTP 状態コードが 500 の場合、応答にはこれらのエラーが含まれます。
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>ブロック|要求の一部が有効でない場合に Bing は InvalidRequest を返します。 たとえば、必要なパラメーターが不足している場合や、パラメーター値が無効な場合です。<br/><br/>エラーが ParameterMissing または ParameterInvalidValue の場合、HTTP 状態コードは 400 です。<br/><br/>エラーが HttpNotAllowed の場合、HTTP 状態コードは 410 です。
|RateLimitExceeded||1 秒あたりのクエリ数 (QPS) または 1 か月あたりのクエリ数 (QPM) のクォータを超えると、Bing は RateLimitExceeded を返します。<br/><br/>QPS を超えた場合、Bing は HTTP状態コード 429 を返し、QPM を超過した場合に 403 を返します。
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing が呼び出し元を認証できない場合、Bing は InvalidAuthorization を返します。 たとえば、`Ocp-Apim-Subscription-Key` ヘッダーが見つからない場合や、サブスクリプション キーが無効な場合などです。<br/><br/>冗長性は、複数の認証方法を指定した場合に発生します。<br/><br/>エラーが InvalidAuthorization の場合、HTTP 状態コードは 401 です。
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|呼び出し元がリソースにアクセスする権限を持っていない場合、Bing は InsufficientAuthorization を返します。 これは、サブスクリプション キーが無効になっているか、期限が切れている場合に発生することがあります。 <br/><br/>エラーが InsufficientAuthorization の場合、HTTP 状態コードは 403 です。

- 次に、以前のエラー コードから新しいコードへのマッピングを示します。 v5 エラー コードに依存していた場合は、コードを適宜更新してください。  
  
|バージョン 5 コード|バージョン 7 コード.サブコード
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
無効|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
ブロック|InvalidRequest.Blocked

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [使用と表示の要件](./UseAndDisplayRequirements.md)
