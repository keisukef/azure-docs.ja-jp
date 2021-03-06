---
title: 'チュートリアル: Azure Active Directory と Recognize の統合 | Microsoft Docs'
description: Azure Active Directory と Recognize の間でシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: jeedes
ms.openlocfilehash: c33b066aa1ba627166021a648a548b9b39d54630
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/19/2018
ms.locfileid: "36219059"
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>チュートリアル: Azure Active Directory と Recognize の統合

このチュートリアルでは、Recognize と Azure Active Directory (Azure AD) を統合する方法について説明します。

Recognize と Azure AD の統合には、次の利点があります。

- Recognize にアクセスする Azure AD ユーザーを制御できます。
- ユーザーが自分の Azure AD アカウントで自動的に Recognize にサインオン (シングル サインオン) できるようにします。
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

Azure AD と Recognize の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- Recognize でのシングル サインオンが有効なサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、[こちら](https://azure.microsoft.com/pricing/free-trial/)から 1 か月の評価版を入手できます。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの Recognize の追加
2. Azure AD シングル サインオンの構成とテスト

## <a name="adding-recognize-from-the-gallery"></a>ギャラリーからの Recognize の追加
Azure AD への Recognize の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に Recognize を追加する必要があります。

**ギャラリーから Recognize を追加するには、次の手順に従います。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Active Directory][1]

2. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![[アプリケーション]][2]
    
3. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![[アプリケーション]][3]

4. 検索ボックスに、「**Recognize**」と入力します。

    ![Azure AD のテスト ユーザーの作成](./media/recognize-tutorial/tutorial_recognize_search.png)

5. 結果ウィンドウで **[Recognize]** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![Azure AD のテスト ユーザーの作成](./media/recognize-tutorial/tutorial_recognize_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、Recognize で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する Recognize ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと Recognize の関連ユーザーの間で、リンク関係が確立されている必要があります。

Recognize で、Azure AD の **[ユーザー名]** の値を **[Username]** の値として割り当ててリンク関係を確立します。

Recognize で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
2. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
3. **[Recognize テスト ユーザーの作成](#creating-a-recognize-test-user)** - Recognize で Britta Simon に対応するユーザーを作成し、Azure AD の Britta Simon にリンクさせます。
4. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
5. **[シングル サインオンのテスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure Portal で Azure AD のシングル サインオンを有効にして、Recognize アプリケーションでシングル サインオンを構成します。

**Recognize で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure Portal の **Recognize** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![[Configure Single Sign-On]][4]

2. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![[Configure Single Sign-On]](./media/recognize-tutorial/tutorial_recognize_samlbase.png)

3. **[Recognize のドメインと URL]** セクションで、次の手順を実行します。

    ![[Configure Single Sign-On]](./media/recognize-tutorial/tutorial_recognize_url.png)

    a. **[サインオン URL]** ボックスに、`https://recognizeapp.com/<your-domain>/saml/sso` のパターンを使用して URL を入力します。

    b. **[識別子]** ボックスに、`https://recognizeapp.com/<your-domain>` の形式で URL を入力します。

    > [!NOTE] 
    > これらは実際の値ではありません。 実際のサインオン URL と識別子でこれらの値を更新してください。 サインオンの URL については、[Recognize クライアント サポート チーム](mailto:support@recognizeapp.com)に問い合わせてください。また、このチュートリアルで後述する [SSO Settings]\(SSO 設定\) セクションのサービス プロバイダー メタデータ URL を開いて識別子の値を取得できます。 が必要です。 
 
4. **[SAML 署名証明書]** セクションで、**[Certificate (Base64) (証明書 (Base64)) ]** をクリックし、コンピューターに証明書ファイルを保存します。

    ![[Configure Single Sign-On]](./media/recognize-tutorial/tutorial_recognize_certificate.png) 

5. **[保存]** ボタンをクリックします。

    ![[Configure Single Sign-On]](./media/recognize-tutorial/tutorial_general_400.png)

6. **[Recognize 構成]** セクションで、**[Recognize の構成]** をクリックして、**[サインオンの構成]** ウィンドウを開きます。 **[クイック リファレンス]** セクションから、**サインアウト URL、SAML エンティティ ID、SAML シングル サインオン サービス URL** をコピーします。

    ![[Configure Single Sign-On]](./media/recognize-tutorial/tutorial_recognize_configure.png) 

7. 別の Web ブラウザーのウィンドウで、管理者として Recognize テナントにサインオンします。

8. 右上にある **[Menu (メニュー)]** をクリックします。 **[Company Admin (会社の管理者)]** に移動します。
   
    ![アプリ側でのシングル サインオンの構成](./media/recognize-tutorial/tutorial_recognize_000.png)

9. 左側のナビゲーション ウィンドウで、 **[Settings (設定)]** をクリックします。
   
    ![アプリ側でのシングル サインオンの構成](./media/recognize-tutorial/tutorial_recognize_001.png)

10. **[SSO Settings (SSO 設定)]** セクションで、次の手順に従います。
   
    ![アプリ側でのシングル サインオンの構成](./media/recognize-tutorial/tutorial_recognize_002.png)
    
    a. **[Enable SSO (SSO を有効にする)]** を **[ON]** にします。

    b. **[IDP Entity ID]\(IDP エンティティ ID\)** ボックスに、Azure Portal からコピーした **SAML エンティティ ID** の値を貼り付けます。
    
    c. **[SSO ターゲット URL]** ボックスに、Azure Portal からコピーした **SAML シングル サインオン サービス URL** の値を貼り付けます。
    
    d. **[Slo target url]\(SLO ターゲット URL\)** ボックスに、Azure Portal からコピーした**サインアウト URL** の値を貼り付けます。 
    
    e. ダウンロードした**証明書 (Base64)** ファイルをメモ帳で開き、その内容をクリップボードにコピーし、**[証明書]** ボックスに貼り付けます。
    
    f. **[Save settings (設定の保存)]** ボタンをクリックします。 

11. **[SSO Settings (SSO 設定)]** セクションに、**Service Provider Metadata url** の URL をコピーします。
   
    ![アプリ側でのシングル サインオンの構成](./media/recognize-tutorial/tutorial_recognize_003.png)

12. 空白のブラウザーで**メタデータ URL リンク**を開き、メタデータ ドキュメントをダウンロードします。 次に、ファイルから EntityDescriptor 値 (entityID) をコピーし、Azure Portal の **[Recognize のドメインと URL]** セクションの **[識別子]** ボックスに貼り付けます。
    
    ![アプリ側でのシングル サインオンの構成](./media/recognize-tutorial/tutorial_recognize_004.png)

> [!TIP]
> アプリのセットアップ中、[Azure Portal](https://portal.azure.com) 内で上記の手順の簡易版を確認できるようになりました。  **[Active Directory] の [エンタープライズ アプリケーション]** セクションからこのアプリを追加した後、**[シングル サインオン]** タブをクリックし、一番下の **[構成]** セクションから組み込みドキュメントにアクセスするだけです。 組み込みドキュメント機能の詳細については、[Azure AD の組み込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関するページを参照してください。
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure Portal** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/recognize-tutorial/create_aaduser_01.png) 

2. **[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/recognize-tutorial/create_aaduser_02.png) 

3. ダイアログの上部にある **[追加]** をクリックして、**[ユーザー]** ダイアログを開きます。
 
    ![Azure AD のテスト ユーザーの作成](./media/recognize-tutorial/create_aaduser_03.png) 

4. **[ユーザー]** ダイアログ ページで、次の手順を実行します。
 
    ![Azure AD のテスト ユーザーの作成](./media/recognize-tutorial/create_aaduser_04.png) 

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに BrittaSimon の**電子メール アドレス**を入力します。

    c. **[パスワードを表示]** を選択し、**[パスワード]** の値をメモします。

    d. **Create** をクリックしてください。
 
### <a name="creating-a-recognize-test-user"></a>Recognize テスト ユーザーの作成

Azure AD ユーザーが Recognize にログインできるようにするには、そのユーザーを Recognize にプロビジョニングする必要があります。 Recognize の場合、プロビジョニングは手動で行います。

このアプリは SCIM プロビジョニングをサポートしていませんが、ユーザーをプロビジョニングする代替ユーザー同期があります。 

**ユーザー アカウントをプロビジョニングするには、次の手順に従います。**

1. Recognize 企業サイトに管理者としてログインします。

2. 右上にある **[Menu (メニュー)]** をクリックします。 **[Company Admin (会社の管理者)]** に移動します。

3. 左側のナビゲーション ウィンドウで、 **[Settings (設定)]** をクリックします。

4. **[User Sync (ユーザー同期)]** セクションで、次の手順に従います。
   
   ![New User](./media/recognize-tutorial/tutorial_recognize_005.png "New User")
   
   a. **[Sync Enabled (同期の有効化)]** で **[ON]** を選択します。
   
   b. **[Choose sync provider (同期プロバイダーの選択)]** で、**[Microsoft / Office 365]** を選択します。
   
   c. **[Run User Sync (ユーザー同期の実行)]** をクリックします。

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に Recognize へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザーの割り当て][200] 

**Recognize に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

2. アプリケーションの一覧で **[Recognize]** を選択します。

    ![[Configure Single Sign-On]](./media/recognize-tutorial/tutorial_recognize_app.png) 

3. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![ユーザーの割り当て][202] 

4. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![ユーザーの割り当て][203]

5. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

6. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

7. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="testing-single-sign-on"></a>シングル サインオンのテスト

このセクションの目的は、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストすることです。

アクセス パネルで [Recognize] タイルをクリックすると、自動的に Recognize アプリケーションにサインオンします。 アクセス パネルの詳細については、[アクセス パネルの概要](../active-directory-saas-access-panel-introduction.md)に関する記事を参照してください。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/recognize-tutorial/tutorial_general_01.png
[2]: ./media/recognize-tutorial/tutorial_general_02.png
[3]: ./media/recognize-tutorial/tutorial_general_03.png
[4]: ./media/recognize-tutorial/tutorial_general_04.png

[100]: ./media/recognize-tutorial/tutorial_general_100.png

[200]: ./media/recognize-tutorial/tutorial_general_200.png
[201]: ./media/recognize-tutorial/tutorial_general_201.png
[202]: ./media/recognize-tutorial/tutorial_general_202.png
[203]: ./media/recognize-tutorial/tutorial_general_203.png

