---
title: 'Didacticiel : Intégration d’Azure Active Directory à Replicon | Microsoft Docs'
description: Découvrez comment configurer l’authentification unique entre Azure Active Directory et Replicon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ade40287bd38580a1e3f6377e54017bfe92bf452
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60527231"
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Didacticiel : Intégration d’Azure Active Directory à Replicon

Dans ce didacticiel, vous allez apprendre à intégrer Replicon à Azure Active Directory (Azure AD).

L’intégration de Replicon à Azure AD vous offre les avantages suivants :

- Dans Azure AD, vous pouvez contrôler qui a accès à Replicon.
- Vous pouvez autoriser les utilisateurs à se connecter automatiquement à Replicon (via l’authentification unique) avec leur compte Azure AD.
- Vous pouvez gérer vos comptes dans un emplacement central : le portail Azure

Pour en savoir plus sur l’intégration des applications SaaS avec Azure AD, consultez [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Conditions préalables

Pour configurer l’intégration d’Azure AD à Replicon, vous avez besoin des éléments suivants :

- Un abonnement Azure AD
- Un abonnement Replicon pour lequel l’authentification unique est activée

> [!NOTE]
> Pour tester les étapes de ce didacticiel, nous déconseillons l’utilisation d’un environnement de production.

Vous devez en outre suivre les recommandations ci-dessous :

- N’utilisez pas votre environnement de production, sauf si cela est nécessaire.
- Si vous n’avez pas d’environnement d’essai Azure AD, vous pouvez [obtenir un essai d’un mois](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Description du scénario
Dans ce didacticiel, vous testez l’authentification unique Azure AD dans un environnement de test. Le scénario décrit dans ce didacticiel se compose des deux sections principales suivantes :

1. Ajout de Replicon à partir de la galerie
2. Configuration et test de l’authentification unique Azure AD

## <a name="adding-replicon-from-the-gallery"></a>Ajout de Replicon à partir de la galerie
Pour configurer l’intégration de Replicon à Azure AD, vous devez ajouter Replicon à partir de la galerie à votre liste d’applications SaaS gérées.

**Pour ajouter Replicon à partir de la galerie, effectuez les étapes suivantes :**

1. Dans le volet de navigation gauche du **[portail Azure](https://portal.azure.com)**, cliquez sur l’icône **Azure Active Directory**.

    ![Bouton Azure Active Directory][1]

2. Accédez à **Applications d’entreprise**. Accédez ensuite à **Toutes les applications**.

    ![Panneau Applications d’entreprise][2]

3. Pour ajouter l’application, cliquez sur le bouton **Nouvelle application** en haut de la boîte de dialogue.

    ![Bouton Nouvelle application][3]

4. Dans la zone de recherche, tapez **Replicon**, sélectionnez **Replicon** dans le panneau de résultats, puis cliquez sur le bouton **Ajouter** pour ajouter l’application.

    ![Replicon dans la liste des résultats](./media/replicon-tutorial/tutorial_replicon_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurer et tester l’authentification unique Azure AD

Dans cette section, vous allez configurer et tester l’authentification unique Azure AD avec Replicon avec un utilisateur de test appelé « Britta Simon ».

Pour que l’authentification unique fonctionne, Azure AD doit savoir qui est l’utilisateur Replicon équivalent dans Azure AD. En d’autres termes, une relation entre un utilisateur Azure AD et un utilisateur Replicon associé doit être établie.

Dans Replicon, affectez la valeur du **nom d’utilisateur** dans Azure AD comme valeur du **nom d’utilisateur** pour établir la relation.

Pour configurer et tester l’authentification unique Azure AD avec Replicon, vous devez suivre les indications des sections suivantes :

1. **[Configurer l’authentification unique Azure AD](#configure-azure-ad-single-sign-on)** pour permettre à vos utilisateurs d’utiliser cette fonctionnalité.
2. **[Créer un utilisateur de test Azure AD](#create-an-azure-ad-test-user)** pour tester l’authentification unique Azure AD avec Britta Simon.
3. **[Créer un utilisateur de test Replicon](#create-a-replicon-test-user)** pour avoir un équivalent de Britta Simon dans Replicon lié à la représentation Azure AD de l’utilisateur.
4. **[Affecter l’utilisateur de test Azure AD](#assign-the-azure-ad-test-user)** pour permettre à Britta Simon d’utiliser l’authentification unique Azure AD.
5. **[Tester l’authentification unique](#test-single-sign-on)** : pour vérifier si la configuration fonctionne.

### <a name="configure-azure-ad-single-sign-on"></a>Configurer l’authentification unique Azure AD

Dans cette section, vous allez activer l’authentification unique Azure AD dans le portail Azure et configurer l’authentification unique dans votre application Replicon.

**Pour configurer l’authentification unique Azure AD avec Replicon, effectuez les étapes suivantes :**

1. Dans le portail Azure, dans la page d’intégration de l’application **Replicon**, cliquez sur **Authentification unique**.

    ![Lien Configurer l’authentification unique][4]

2. Dans la boîte de dialogue **Authentification unique**, pour le **Mode**, sélectionnez **Authentification basée sur SAML** pour activer l’authentification unique.

    ![Boîte de dialogue Authentification unique](./media/replicon-tutorial/tutorial_replicon_samlbase.png)

3. Dans la section **Domaine et URL Replicon**, effectuez les étapes suivantes :

    ![Informations d’authentification unique dans Domaine et URL Replicon](./media/replicon-tutorial/tutorial_replicon_url.png)

    a. Dans la zone de texte **URL de connexion**, tapez une URL au format suivant : `https://na2.replicon.com/<companyname>/saml2/sp-sso/post`

    b. Dans la zone de texte **Identificateur**, tapez une URL au format suivant : `https://global.replicon.com/<companyname>`

    c. Dans la zone de texte **URL de réponse** , tapez une URL au format suivant : `https://global.replicon.com/!/saml2/<companyname>/sso/post`

    > [!NOTE]
    > Il ne s’agit pas de valeurs réelles. Mettez à jour ces valeurs avec l’URL de connexion, l’identificateur et l’URL de réponse réels. Pour obtenir ces valeurs, contactez [l’équipe de support technique Replicon](https://www.replicon.com/customerzone/contact-support). 

4. Dans la section **Certificat de signature SAML**, cliquez sur **Métadonnées XML** puis enregistrez le fichier de métadonnées sur votre ordinateur.

    ![Lien Téléchargement de certificat](./media/replicon-tutorial/tutorial_replicon_certificate.png) 

5. Cliquez sur le bouton **Enregistrer** .

    ![Bouton Enregistrer de la page Configurer l’authentification unique](./media/replicon-tutorial/tutorial_general_400.png)

6. Dans une autre fenêtre de navigateur web, connectez-vous à votre site d’entreprise Replicon en tant qu’administrateur.

7. Procédez comme suit pour configurer SAML 2.0 :

    ![Activer l’authentification SAML](./media/replicon-tutorial/ic777805.png "Activer l’authentification SAML")

    a. Pour afficher la boîte de dialogue **EnableSAML Authentication2**, ajoutez ce qui suit à votre URL, après la clé de votre entreprise : `/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * Voici une illustration du schéma de l’URL complète : `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. Cliquez sur **+** pour développer la section **v20Configuration**.

   c. Cliquez sur **+** pour développer la section **metaDataConfiguration**.

   d. Cliquez sur **Choose File** pour sélectionner votre fichier XML de métadonnées de fournisseur d’identité, puis cliquez sur **Submit**.

### <a name="create-an-azure-ad-test-user"></a>Créer un utilisateur de test Azure AD

L’objectif de cette section est de créer un utilisateur de test appelé Britta Simon dans le portail Azure.

   ![Créer un utilisateur de test Azure AD][100]

**Pour créer un utilisateur de test dans Azure AD, procédez comme suit :**

1. Dans le volet gauche du Portail Azure, cliquez sur le bouton **Azure Active Directory**.

    ![Bouton Azure Active Directory](./media/replicon-tutorial/create_aaduser_01.png)

2. Pour afficher la liste des utilisateurs, accédez à **Utilisateurs et groupes**, puis cliquez sur **Tous les utilisateurs**.

    ![Liens « Utilisateurs et groupes » et « Tous les utilisateurs »](./media/replicon-tutorial/create_aaduser_02.png)

3. Pour ouvrir la boîte de dialogue **Utilisateur**, cliquez sur **Ajouter** en haut de la boîte de dialogue **Tous les utilisateurs**.

    ![Bouton Ajouter](./media/replicon-tutorial/create_aaduser_03.png)

4. Dans la boîte de dialogue **Utilisateur**, procédez comme suit :

    ![Boîte de dialogue Utilisateur](./media/replicon-tutorial/create_aaduser_04.png)

    a. Dans la zone **Nom**, tapez **BrittaSimon**.

    b. Dans la zone **Nom d’utilisateur** , tapez l’adresse e-mail de l’utilisateur Britta Simon.

    c. Cochez la case **Afficher le mot de passe**, puis notez la valeur affichée dans le champ **Mot de passe**.

    d. Cliquez sur **Créer**.

### <a name="create-a-replicon-test-user"></a>Créer un utilisateur de test Replicon

L’objectif de cette section est de créer un utilisateur appelé Britta Simon dans Replicon.

**Si vous avez besoin de créer un utilisateur manuellement, procédez comme suit :**

1. Dans une fenêtre de navigateur web, connectez-vous à votre site d’entreprise Replicon en tant qu’administrateur.

2. Accédez à **Administration \> Users**.

    ![Utilisateurs](./media/replicon-tutorial/ic777806.png "Utilisateurs")

3. Cliquez sur **+Add User**.

    ![Ajouter un utilisateur](./media/replicon-tutorial/ic777807.png "Ajouter un utilisateur")

4. Dans la section **User Profile** , procédez comme suit :

    ![Profil utilisateur](./media/replicon-tutorial/ic777808.png "Profil utilisateur")

    a. Dans le **Nom_compte_de_connexion** zone de texte, adresse de messagerie de type Azure AD de l’utilisateur Azure AD que vous souhaitez configurer comme **BrittaSimon\@contoso.com**.

    b. Pour **Authentication Type**, sélectionnez **SSO**.

    c. Dans la zone de texte **Department** , tapez le département de l’utilisateur.

    d. Pour **Employee Type**, sélectionnez **Administrator**.

    e. Cliquez sur **Save User Profile**.

>[!NOTE]
>Vous pouvez utiliser n’importe quel outil ou API de création de compte utilisateur, fourni par Replicon, pour provisionner des comptes utilisateur Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Affecter l’utilisateur de test Azure AD

Dans cette section, vous allez autoriser Britta Simon à utiliser l’authentification unique Azure en lui accordant l’accès à Replicon.

![Attribuer le rôle utilisateur][200]

**Pour affecter Britta Simon à Replicon, effectuez les étapes suivantes :**

1. Dans le portail Azure, ouvrez la vue des applications, accédez à la vue des répertoires, accédez à **Applications d’entreprise**, puis cliquez sur **Toutes les applications**.

    ![Affecter des utilisateurs][201]

2. Dans la liste des applications, sélectionnez **Replicon**.

    ![Lien Replicon dans la liste des applications](./media/replicon-tutorial/tutorial_replicon_app.png)

3. Dans le menu de gauche, cliquez sur **Utilisateurs et groupes**.

    ![Lien « Utilisateurs et groupes »][202]

4. Cliquez sur le bouton **Ajouter**. Ensuite, sélectionnez **Utilisateurs et groupes** dans la boîte de dialogue **Ajouter une affectation**.

    ![Volet Ajouter une attribution][203]

5. Dans la boîte de dialogue **Utilisateurs et groupes**, sélectionnez **Britta Simon** dans la liste des utilisateurs.

6. Cliquez sur le bouton **Sélectionner** dans la boîte de dialogue **Utilisateurs et groupes**.

7. Cliquez sur le bouton **Affecter** dans la boîte de dialogue **Ajouter une affectation**.

### <a name="test-single-sign-on"></a>Tester l’authentification unique

Dans cette section, vous allez tester la configuration de l’authentification unique Azure AD à l’aide du volet d’accès.

Quand vous cliquez sur la vignette Replicon dans le volet d’accès, vous devez être connecté automatiquement à votre application Replicon.
Pour plus d’informations sur le panneau d’accès, consultez [Présentation du panneau d’accès](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Ressources supplémentaires

* [Liste de didacticiels sur l’intégration d’applications SaaS avec Azure Active Directory](tutorial-list.md)
* [Qu’est-ce que l’accès aux applications et l’authentification unique avec Azure Active Directory ?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/replicon-tutorial/tutorial_general_01.png
[2]: ./media/replicon-tutorial/tutorial_general_02.png
[3]: ./media/replicon-tutorial/tutorial_general_03.png
[4]: ./media/replicon-tutorial/tutorial_general_04.png

[100]: ./media/replicon-tutorial/tutorial_general_100.png

[200]: ./media/replicon-tutorial/tutorial_general_200.png
[201]: ./media/replicon-tutorial/tutorial_general_201.png
[202]: ./media/replicon-tutorial/tutorial_general_202.png
[203]: ./media/replicon-tutorial/tutorial_general_203.png
