---
title: 'Démarrage rapide : Obtenir des insights sur les images à l’aide de l’API REST Recherche visuelle Bing et de Java'
titleSuffix: Azure Cognitive Services
description: Découvrez comment charger une image dans l’API Recherche visuelle Bing pour récupérer des insights sur celle-ci.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 2fe4e9dad0b198fe54e06ce07100d231f1f7d157
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59046442"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-java"></a>Démarrage rapide : Obtenir des insights sur les images à l’aide de l’API REST Recherche visuelle Bing et de Java

Utilisez ce guide de démarrage rapide pour effectuer votre premier appel à l’API Recherche visuelle Bing et afficher les résultats. Cette application Java charge une image dans l’API et affiche les informations qu’elle retourne. Bien que cette application soit écrite en Java, l’API est un service web RESTful compatible avec la plupart des langages de programmation.

Quand vous chargez une image locale, les données de formulaire doivent inclure l’en-tête `Content-Disposition`. Vous devez définir son paramètre `name`sur « image », et vous pouvez définir le paramètre `filename` sur n’importe quelle chaîne. Le contenu du formulaire inclut les données binaires de l’image. La taille maximale de l’image que vous chargez est de 1 Mo.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Prérequis

* Le [JDK (Java Development Kit) 7 ou 8](https://aka.ms/azure-jdks)
* La [bibliothèque Gson Java](https://github.com/google/gson)
* [Apache HttpComponents](https://hc.apache.org/downloads.cgi)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Créer et initialiser un projet

1. Créez un projet Java dans votre éditeur ou votre IDE favori, puis importez les bibliothèques suivantes :

    ```java
    import java.util.*;
    import java.io.*;
    import com.google.gson.Gson;
    import com.google.gson.GsonBuilder;
    import com.google.gson.JsonObject;
    import com.google.gson.JsonParser;
    
    // HttpClient libraries
    
    import org.apache.http.HttpEntity;
    import org.apache.http.HttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.ContentType;
    import org.apache.http.entity.mime.MultipartEntityBuilder;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClientBuilder;
    ```

2. Créez des variables pour le point de terminaison de l’API, la clé d’abonnement et le chemin de votre image :

    ```java
    static String endpoint = "https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch";
    static String subscriptionKey = "your-key-here";
    static String imagePath = "path-to-your-image";
    ```

## <a name="create-the-json-parser"></a>Créer l’analyseur JSON

Créez une méthode pour rendre la réponse JSON de l’API plus lisible à l’aide de `JsonParser` :

    ```java
    public static String prettify(String json_text) {
            JsonParser parser = new JsonParser();
            JsonObject json = parser.parse(json_text).getAsJsonObject();
            Gson gson = new GsonBuilder().setPrettyPrinting().create();
            return gson.toJson(json);
        }
    ```

## <a name="construct-the-search-request-and-query"></a>Construire la requête de recherche et la demande

1. Dans la méthode main de votre application, créez un client HTTP à l’aide de `HttpClientBuilder.create().build();` :

    ```java
    CloseableHttpClient httpClient = HttpClientBuilder.create().build();
    ```

2. Créez un objet `HttpEntity` pour charger votre image dans l’API :

    ```java
    HttpEntity entity = MultipartEntityBuilder
        .create()
        .addBinaryBody("image", new File(imagePath))
        .build();
    ```

3. Créez un objet `httpPost` avec votre point de terminaison et définissez l’en-tête pour qu’elle utilise votre clé d’abonnement :

    ```java
    HttpPost httpPost = new HttpPost(endpoint);
    httpPost.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
    httpPost.setEntity(entity);
    ```

## <a name="receive-and-process-the-json-response"></a>Recevoir et traiter la réponse JSON

1. Utilisez la méthode `HttpClient.execute()` pour envoyer une demande à l’API et stockez la réponse dans un objet `InputStream` :
    
    ```java
    HttpResponse response = httpClient.execute(httpPost);
    InputStream stream = response.getEntity().getContent();
    ```

2. Stockez la chaîne JSON et affichez la réponse :

```java
String json = new Scanner(stream).useDelimiter("\\A").next();
System.out.println("\nJSON Response:\n");
System.out.println(prettify(json));
```

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Générer une application web monopage de recherche visuelle](../tutorial-bing-visual-search-single-page-app.md)
