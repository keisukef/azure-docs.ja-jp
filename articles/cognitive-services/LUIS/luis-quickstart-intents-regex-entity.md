---
title: 'チュートリアル: LUIS アプリを作成して正規表現と一致するデータを取得する - Azure | Microsoft Docs'
description: このチュートリアルでは、意図と正規表現のエンティティを使用して簡単な LUIS アプリを作成し、データを抽出する方法を学習します。
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: v-geberr
ms.openlocfilehash: 522d24c1c03a338633c340502087300c890d1771
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/29/2018
ms.locfileid: "37128447"
---
# <a name="tutorial-3-add-regular-expression-entity"></a>チュートリアル: 3. 正規表現エンティティを追加する
このチュートリアルでは、**正規表現**のエンティティを使用して、発話から一貫した書式のデータを抽出する方法を示すアプリを作成します。


<!-- green checkmark -->
> [!div class="checklist"]
> * 正規表現のエンティティを理解する 
> * FindForm 意図で人事 (HR) ドメインの LUIS アプリを使用する
> * 正規表現エンティティを追加して、発話からフォーム番号を抽出する
> * アプリをトレーニングして、公開する
> * アプリのエンドポイントをクエリして LUIS JSON の応答を表示する

この記事に従って LUIS アプリケーションを作成するには、無料の [LUIS](luis-reference-regions.md#luis-website) アカウントが必要です。

## <a name="before-you-begin"></a>開始する前に
[事前構築済みエンティティ](luis-tutorial-prebuilt-intents-entities.md) チュートリアルの人事アプリがない場合は、[LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json) GitHub リポジトリから JSON を [LUIS](luis-reference-regions.md#luis-website) Web サイトの新しいアプリに[インポート](create-new-app.md#import-new-app)します。

元の人事アプリを保持したい場合は、[[設定]](luis-how-to-manage-versions.md#clone-a-version) ページ上でバージョンを複製して、`regex` という名前を付けます。 複製は、元のバージョンに影響を及ぼさずにさまざまな LUIS 機能を使用するための優れた方法です。 


## <a name="purpose-of-the-regular-expression-entity"></a>正規表現のエンティティの目的
エンティティの目的は、発話に含まれている重要なデータを取得することです。 正規表現のエンティティのアプリを使用することで、書式設定された人事 (HR) のフォーム番号を発話から抽出します。 機械学習が行われるわけではありません。 

簡単な発話の例を次に示します。

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

発話の省略形または俗語バージョンを次に示します。

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
フォーム番号と一致する正規表現のエンティティは `hrf-[0-9]{6}` です。 この正規表現はリテラル文字の `hrf -` と一致しますが、大文字小文字およびカルチャのバリアントは無視します。 0 ～ 9 の 6 桁の数字と正確に一致します。

HRF は人事フォームを意味します。

### <a name="tokenization-with-hyphens"></a>ハイフンを使ったトークン化
LUIS では、発話が意図に追加されるときに、発話をトークン化します。 これらの発話のトークン化では、`Where is HRF - 123456?` のように、ハイフンの前後にスペースを追加します。トークン化される前に、正規表現が raw 形式の発話に適用されます。 _raw_ 形式に適用されているため、正規表現で単語の境界を処理する必要はありません。 


## <a name="add-findform-intent"></a>FindForm 意図を追加する

1. 人事アプリは必ず、LUIS の**ビルド** セクションに配置してください。 右上のメニュー バーにある **[ビルド]** を選択すると、このセクションに変更できます。 

    [ ![右上のナビゲーション バーにある [ビルド] が強調表示された LUIS アプリのスクリーンショット](./media/luis-quickstart-intents-regex-entity/first-image.png)](./media/luis-quickstart-intents-regex-entity/first-image.png#lightbox)

2. **[Create new intent]\(意図の新規作成\)** を選択します。 

    [ ![[Create new intent]\(意図の新規作成\) ボタンが強調表示されている [Intents]\(意図\) ページのスクリーンショット](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png) ](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png#lightbox)

3. ポップアップ ダイアログ ボックスに「`FindForm`」と入力して、**[完了]** を選択します。 

    ![検索ボックスにユーティリティが表示されている [Create new intent]\(意図の新規作成\) ダイアログのスクリーンショット](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. 発話の例を意図に追加します。

    |発話の例|
    |--|
    |hrf-123456 の URL は何ですか。|
    |hrf-345678 の場所はどこですか。|
    |hrf-456098 はいつ更新されましたか。|
    |John Smith は先週、hrf-234639 を更新しましたか。|
    |hrf-345123 のバージョン数はいくつですか。|
    |フォーム hrf-123456 を誰が承認する必要がありますか。|
    |hrf 345678 をサインオフする必要があるユーザー数はいくつですか。|
    |hrf-234123 の日付はいつですか。|
    |hrf 546234 の作成者は誰ですか。|
    |hrf-456234 のタイトルは何ですか。|

    [ ![新しい発話が強調表示されている [Intents]\(意図\) ページのスクリーンショット](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    アプリケーションには、前のチュートリアルで追加された事前構築済みの番号エンティティがあるため、各フォーム番号がタグ付けされています。 クライアント アプリケーションにとってはこれで十分な場合がありますが、番号は、番号の種類と共にラベル付けされません。 適切な名前で新しいエンティティを作成すると、クライアント アプリケーションは、LUIS から返されたときに適切にエンティティを処理できます。

## <a name="create-a-hrf-number-regular-expression-entity"></a>HRF 番号の正規表現エンティティの作成 
次の手順で、正規表現エンティティを作成して、HRF 番号形式を LUIS に認識させます。

1. 左のパネルで **[エンティティ]** を選びます。

2. [エンティティ] ページで **[新しいエンティティの作成]** ボタンを選択します。 

    [ ![[新しいエンティティの作成] ボタンが強調表示されている [エンティティ] ページのスクリーンショット](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png)](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png#lightbox)

3. ポップアップ ダイアログで、新しいエンティティ名 `HRF-number` を入力して、エンティティ型として **RegEx** を選択し、Regex に「`hrf-[0-9]{6}`」と入力してから、**[完了]** を選択します。

    ![ポップアップ ダイアログ設定の新しいエンティティ プロパティのスクリーン ショット](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. **[Intents]\(意図\)**、**[FindForm]** 意図の順に選択して、発話に正規表現のラベルが付与されていることを確認します。 

    [![既存のエンティティと正規パターンでのラベル発話のスクリーンショット](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    エンティティは機械学習エンティティではないため、ラベルは、作成されるとすぐに発話に適用されて LUIS Web サイトに表示されます。

## <a name="train-the-luis-app"></a>LUIS アプリをトレーニングする
正規表現のエンティティはトレーニングを必要としませんが、新しい意図と発話ではトレーニングが必要になります。 

1. LUIS Web サイトの右上にある **[Train]\(トレーニング\)** ボタンを選択します。

    ![[Train]\(トレーニング\) ボタンの画像](./media/luis-quickstart-intents-regex-entity/train-button.png)

2. 成功したことを示す緑色のステータス バーが Web サイトの上部に表示されたら、トレーニングは完了しています。

    ![成功の通知バーの画像](./media/luis-quickstart-intents-regex-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>アプリを公開してエンドポイント URL を取得する
チャットボットや他のアプリケーションで LUIS の予測を取得するには、アプリを公開する必要があります。 

1. LUIS Web サイトの右上にある **[Publish]\(公開\)** ボタンを選択します。 

    ![上部のナビゲーションにある [Publish]\(公開\) ボタンが強調表示された FindKnowledgeBase のスクリーンショット](./media/luis-quickstart-intents-regex-entity/publish-button.png)

2. [Production]\(運用\) スロットを選択し、**[Publish]\(公開\)** ボタンを選択します。

    ![運用スロットへの [Publish]\(公開\) ボタンが強調表示された [公開] ページのスクリーンショット](./media/luis-quickstart-intents-regex-entity/publish-to-production.png)

3. 成功したことを示す緑色のステータス バーが Web サイトの上部に表示されたら、公開は完了しています。

## <a name="query-the-endpoint-with-a-different-utterance"></a>異なる発話でエンドポイントにクエリを行う
1. **[Publish]\(公開\)** ページで、ページの下部にある**エンドポイント**のリンクを選択します。 別のブラウザー ウィンドウが開き、アドレス バーにエンドポイント URL が表示されます。 

    ![エンドポイントの URL が強調表示された [Publish]\(公開\) ページのスクリーンショット](./media/luis-quickstart-intents-regex-entity/publish-select-endpoint.png)

2. アドレスの URL の末尾に移動し、「`When were HRF-123456 and hrf-234567 published in the last year?`」と入力します。 最後の querystring パラメーターは `q` です。これは発話の**クエリ**です。 この発話はラベル付けされたどの発話とも同じではないので、よいテストであり、`FindForm` 意図と 2 つのフォーム番号 `HRF-123456` と `hrf-234567` が返される必要があります。

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    発話の番号が 2 回返されます。1 回は新しいエンティティの `hrf-number` として、もう 1 回は構築済みエンティティの `number` としてです。 この例に示すように、発話は複数のエンティティを保持でき、同じ種類のエンティティが複数あってもかまいません。 正規表現のエンティティを使用することで、LUIS は名前付けされたデータを抽出します。このデータは、JSON 応答を受信するクライアント アプリケーションにとって、プログラムでより有益です。

## <a name="what-has-this-luis-app-accomplished"></a>この LUIS アプリの処理内容
このアプリでは意図を識別し、抽出したデータを返しました。 

チャットボットは、基本アクションの `FindForm` および検索に入力されたフォーム番号を判断できる十分な情報を得ました。 

## <a name="where-is-this-luis-data-used"></a>この LUIS データの使用場所 
LUIS はこの要求の処理を完了しています。 チャットボットなどの呼び出し側アプリでは、topScoringIntent 結果とフォーム番号を取得して、サードパーティ製の API を検索できます。 LUIS ではその作業を行いません。 LUIS はユーザーの意図を判断して、その意図に関する情報を抽出するだけです。 

## <a name="clean-up-resources"></a>リソースのクリーンアップ
不要になったら、LUIS アプリを削除します。 左上のメニューで **[マイ アプリ]** を選択します。 アプリ リストのアプリ名の右にある 3 つのドット メニュー [...] を選択し、**[削除]** を選択します。 **[Delete app?]\(アプリを削除しますか?\)** のポップアップ ダイアログで、**[OK]** を選択します。

## <a name="next-steps"></a>次の手順

> [!div class="nextstepaction"]
> [リスト エンティティについて学習する](luis-quickstart-intent-and-list-entity.md)

