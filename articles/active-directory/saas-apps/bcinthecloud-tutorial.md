---
title: 'チュートリアル: Azure Active Directory と BC in the Cloud の統合 | Microsoft Docs'
description: Azure Active Directory と BC in the Cloud の間でシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 7dc40d2c-6349-40cb-b304-b098bd03a66c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/1/2017
ms.author: jeedes
ms.openlocfilehash: 936587b5f1cd90104073438232141a113cabb375
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36226784"
---
# <a name="tutorial-azure-active-directory-integration-with-bc-in-the-cloud"></a>チュートリアル: Azure Active Directory と BC in the Cloud の統合

このチュートリアルでは、BC in the Cloud と Azure Active Directory (Azure AD) を統合する方法について説明します。

BC in the Cloud と Azure AD の統合には、次の利点があります。

- BC in the Cloud にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に BC in the Cloud にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

BC in the Cloud と Azure AD の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- BC in the Cloud でのシングル サインオンが有効なサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、 [こちら](https://azure.microsoft.com/pricing/free-trial/)から 1 か月の評価版を入手できます。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの BC in the Cloud の追加
2. Azure AD シングル サインオンの構成とテスト

## <a name="adding-bc-in-the-cloud-from-the-gallery"></a>ギャラリーからの BC in the Cloud の追加
BC in the Cloud の Azure AD への統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に、BC in the Cloud を追加する必要があります。

**ギャラリーから BC in the Cloud を追加するには、次の手順に従います。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Active Directory][1]

2. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![[アプリケーション]][2]
    
3. 新しいアプリケーションを追加するには、ダイアログの上部にある **[追加]** をクリックします。

    ![[アプリケーション]][3]

4. 検索ボックスに、「**BC in the Cloud**」と入力します。

    ![Azure AD のテスト ユーザーの作成](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_search.png)

5. 結果ウィンドウで **[BC in the Cloud]** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![Azure AD のテスト ユーザーの作成](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、BC in the Cloud で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する BC in the Cloud ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと BC in the Cloud の関連ユーザーの間で、リンク関係が確立されている必要があります。

BC in the Cloud で、Azure AD の **[ユーザー名]** の値を **[ユーザー名]** の値として割り当ててリンク関係を確立します。

BC in the Cloud での Azure AD シングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[BC in the Cloud のテスト ユーザーの作成](#creating-a-bc-in-the-cloud-test-user)** - BC in the Cloud で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
5. **[シングル サインオンのテスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure Portal で Azure AD のシングル サインオンを有効にして、BC in the Cloud アプリケーションでシングル サインオンを構成します。

**BC in the Cloud との Azure AD シングル サインオンを構成するには、次の手順を実行します。**

1. Azure Portal の **BC in the Cloud** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![[Configure Single Sign-On]][4]

2. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![[Configure Single Sign-On]](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_samlbase.png)

3. **[BC in the Cloud Domain and URLs]\(BC in the Cloud のドメインと URL\)** セクションで、次の手順に従います。

    ![[Configure Single Sign-On]](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_url.png)

    a. **[サインオン URL]** ボックスに、`https://app.bcinthecloud.com/router/loginSaml/<customerid>` のパターンを使用して URL を入力します。

    b. **[識別子]** ボックスに、`https://app.bcinthecloud.com` のように URL を入力します。

    > [!NOTE] 
    > これは実際の値ではありません。 実際のサインオン URL でこの値を更新してください。 この値を取得するには、[BC in the Cloud クライアント サポート チーム](https://www.bcinthecloud.com/supportcenter/)に問い合わせてください。 
 
4. **[SAML 署名証明書]** セクションで、**[Metadata XML (メタデータ XML)]** をクリックし、コンピューターにメタデータ ファイルを保存します。

    ![[Configure Single Sign-On]](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_certificate.png) 

5. **[保存]** ボタンをクリックします。

    ![[Configure Single Sign-On]](./media/bcinthecloud-tutorial/tutorial_general_400.png)

6. **BC in the Cloud** 側にシングル サインオンを構成するには、ダウンロードした**メタデータ XML** を [BC in the Cloud サポート チーム](https://www.bcinthecloud.com/supportcenter/)に送信する必要があります。

> [!TIP]
> アプリのセットアップ中、[Azure Portal](https://portal.azure.com) 内で上記の手順の簡易版を確認できるようになりました。  **[Active Directory] の [エンタープライズ アプリケーション]** セクションからこのアプリを追加した後、**[シングル サインオン]** タブをクリックし、一番下の **[構成]** セクションから組み込みドキュメントにアクセスするだけです。 組み込みドキュメント機能の詳細については、[Azure AD の組み込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関するページを参照してください。
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure Portal** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/bcinthecloud-tutorial/create_aaduser_01.png) 

2. **[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/bcinthecloud-tutorial/create_aaduser_02.png) 

3. ダイアログの上部にある **[追加]** をクリックして、**[ユーザー]** ダイアログを開きます。
 
    ![Azure AD のテスト ユーザーの作成](./media/bcinthecloud-tutorial/create_aaduser_03.png) 

4. **[ユーザー]** ダイアログ ページで、次の手順を実行します。
 
    ![Azure AD のテスト ユーザーの作成](./media/bcinthecloud-tutorial/create_aaduser_04.png) 

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに BrittaSimon の**電子メール アドレス**を入力します。

    c. **[パスワードを表示]** を選択し、**[パスワード]** の値をメモします。

    d. **Create** をクリックしてください。
 
### <a name="creating-a-bc-in-the-cloud-test-user"></a>BC in the Cloud のテスト ユーザーの作成

このセクションでは、BC in the Cloud で Britta Simon というユーザーを作成します。 [BC in the Cloud クライアント サポート チーム](https://www.bcinthecloud.com/supportcenter/)と連携して、BC in the Cloud アプリケーションにユーザーを追加します。 シングル サインオンを使用する前に、ユーザーを作成し、有効化する必要があります。 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に BC in the Cloud へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザーの割り当て][200] 

**BC in the Cloud に Britta Simon を割り当てるには、次の手順を実行します。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

2. アプリケーションの一覧で **[BC in the Cloud]** を選択します。

    ![[Configure Single Sign-On]](./media/bcinthecloud-tutorial/tutorial_bcinthecloud_app.png) 

3. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![ユーザーの割り当て][202] 

4. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![ユーザーの割り当て][203]

5. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

6. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

7. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="testing-single-sign-on"></a>シングル サインオンのテスト

このセクションでは、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストします。

 アクセス パネルで BC in the Cloud のタイルをクリックすると、自動的に BC in the Cloud アプリケーションにサインオンします。 アクセス パネルの詳細については、[アクセス パネルの概要](../active-directory-saas-access-panel-introduction.md)に関する記事を参照してください。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/bcinthecloud-tutorial/tutorial_general_01.png
[2]: ./media/bcinthecloud-tutorial/tutorial_general_02.png
[3]: ./media/bcinthecloud-tutorial/tutorial_general_03.png
[4]: ./media/bcinthecloud-tutorial/tutorial_general_04.png

[100]: ./media/bcinthecloud-tutorial/tutorial_general_100.png

[200]: ./media/bcinthecloud-tutorial/tutorial_general_200.png
[201]: ./media/bcinthecloud-tutorial/tutorial_general_201.png
[202]: ./media/bcinthecloud-tutorial/tutorial_general_202.png
[203]: ./media/bcinthecloud-tutorial/tutorial_general_203.png

