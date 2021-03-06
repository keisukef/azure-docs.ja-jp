---
title: Text to Speech について | Microsoft Docs
description: Text to Speech の機能の概要。
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: d7ec8648a8428558264c9bfd4d923523b90cce07
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855942"
---
# <a name="about-the-text-to-speech-api"></a>Text to Speech API について

Speech サービスの **Text to Speech** (TTS) API では、入力テキストを自然な音声の発話 (”*音声合成*” とも呼ばれる) に変換します。

音声を生成するために、アプリケーションから Speech サービスに HTTP POST 要求が送信されます。 そこで、テキストは人の発する音声に合成され、オーディオ ファイルとして返されます。 さまざまな音声と言語がサポートされています。

音声合成が採用されているシナリオには次のようなものがあります。

* *アクセシビリティの向上:* **Text to Speech** テクノロジを使用すると、コンテンツの所有者および発行者は、ユーザーがコンテンツを操作するためのさまざまな方法に対応することができます。 視覚に障碍のある人または読み取りが難しい人は、コンテンツを聴覚的に使用することができると助かります。 また、音声出力により、ユーザーは通勤中や運動中にモバイル デバイス上で新聞やブログなどのテキスト コンテンツを容易に利用できるようになります。

* *マルチタスク シナリオでの応答:* **Text to Speech** により、ユーザーは、運転中あるいは読み取りが容易でない環境にいるときでも、重要な情報を迅速かつ快適に吸収することができます。 この領域での一般的な用途としてナビゲーションがあります。 

* *複数のモードでの学習の強化:* さまざまな人がさまざまな方法で最適に学習することができます。 オンライン学習の専門家は、音声とテキストが一緒に提供されれば、情報をより簡単に学習および保持できることを示しています。

* *直感的なボットまたはアシスタントの配信:* 対話機能は、インテリジェント チャット ボットまたは仮想アシスタントに不可欠な部分になります。 顧客に魅力的なカスタマー サービス エクスペリエンスを提供するチャット ボットを開発する企業はますます増えています。 ボットの応答を聴覚的に受信できるようにすることで (たとえば、電話によって)、音声によって別の側面が追加されます。

## <a name="voice-support"></a>音声のサポート

Microsoft **Text-to-Speech** サービスでは、45 を超える言語およびロケールで 75 を超える音声を提供しています。 このような標準の ”音声フォント” を使用するには、サービスの REST API を呼び出すときに、他のいくつかのパラメーターで音声名を指定する必要があるだけです。 サポートされている音声の詳細については、[サポートされている言語](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/supported-languages#text-to-speech)に関するページを参照してください。 

独自の音声をアプリケーションで使用したい場合は、独自の音声サンプルから[カスタム音声フォント](how-to-customize-voice-font.md)を作成できます。

## <a name="next-steps"></a>次の手順

* [Speech 試用版サブスクリプションを取得する](https://azure.microsoft.com/try/cognitive-services/)
* [C# で音声を認識する方法を確認する](quickstart-csharp-windows.md)
