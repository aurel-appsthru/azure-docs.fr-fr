---
title: Comprendre la prise en charge d’Azure IoT Hub AMQP | Microsoft Docs
description: Guide du développeur - prise en charge des appareils qui se connectent à IoT Hub des points de terminaison côté appareil et côté service en utilisant le protocole AMQP. Inclut des informations sur AMQP prise en charge dans les kits Azure IoT device SDK intégrée.
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: rezas
ms.openlocfilehash: 703e2c842fb42bad8aa112d84c516a29c2327378
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473505"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>Communiquer avec votre IoT hub à l’aide du protocole AMQP

IoT Hub prend en charge [AMQP version 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) de proposer un éventail de fonctionnalités via les points de terminaison côté appareil et côté service. Ce document décrit l’utilisation de clients AMQP pour se connecter à IoT Hub pour pouvoir utiliser la fonctionnalité IoT Hub.

## <a name="service-client"></a>Client de service

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Connexion et l’authentification auprès d’IoT Hub (client de service)
Pour vous connecter à IoT Hub à l’aide d’AMQP, un client peut utiliser le [sécurité en fonction des revendications (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) ou [authentification Simple Authentication and Security Layer SASL ()](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

Les informations suivantes sont requises pour le client du service :

| Information | Valeur | 
|-------------|--------------|
| Nom d'hôte IoT Hub | `<iot-hub-name>.azure-devices.net` |
| Nom de la clé | `service` |
| Clé d’accès | Clé primaire ou secondaire associée au service |
| Signature d’accès partagé | SAP de courte durée de vie dans le format suivant : `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}` (le code pour générer cette signature peut être trouvé [ici](./iot-hub-devguide-security.md#security-token-structure)).


L’extrait de code ci-dessous utilise [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python) pour se connecter à IoT hub via un lien de l’expéditeur.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # e.g., '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoking-cloud-to-device-messages-service-client"></a>Appel de messages cloud-à-appareil (client de service)
L’échange de messages de cloud-à-appareil entre service et IoT Hub, ainsi qu’entre appareil et IoT Hub est décrite [ici](iot-hub-devguide-messages-c2d.md). Le client de service utilise deux liens décrites ci-dessous pour envoyer des messages et recevoir des commentaires pour les messages déjà envoyés à partir d’appareils.

| Créé par | Type de lien | Chemin d’accès du lien | Description  |
|------------|-----------|-----------|-------------|
| de diffusion en continu | Lien de l’expéditeur | `/messages/devicebound` | Messages C2D destinés aux appareils sont envoyés à ce lien par le service. Les messages envoyés sur ce lien ont leur `To` propriété définie sur le chemin de lien du récepteur de l’appareil cible : par exemple, `/devices/<deviceID>/messages/devicebound`. |
| de diffusion en continu | Lien du récepteur | `/messages/serviceBound/feedback` | Saisie semi-automatique, de rejet et d’abandon des commentaires les messages provenant des appareils reçus sur ce lien par service. Consultez [ici](./iot-hub-devguide-messages-c2d.md#message-feedback) pour plus d’informations sur les messages de commentaires. |

L’extrait de code ci-dessous montre comment créer un message C2D et l’envoyer à un appareil à l’aide [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the devicebound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message using the send client created and connected IoT Hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if not needed
send_client.close()
```

Pour recevoir des commentaires, client de service crée un lien du récepteur. L’extrait de code ci-dessous montre comment effectuer cette opération à l’aide de [bibliothèque uAMQP dans Python](https://github.com/Azure/azure-uamqp-python).

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Recreate the URI using the feedback path above and authenticate
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
  print('received a message')
  # Check content_type in message property to identify feedback messages coming from device
  if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
    msg_body_raw = msg.get_data()
    msg_body_str = ''.join(msg_body_raw)
    msg_body = json.loads(msg_body_str)
    print(json.dumps(msg_body, indent=2))
    print('******************')
    for feedback in msg_body:
      print('feedback received')
      print('\tstatusCode: ' + str(feedback['statusCode']))
      print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
      print('\tdeviceId: ' + str(feedback['deviceId']))
      print
  else:
    print('unknown message:', msg.properties.content_type)
```

Comme indiqué ci-dessus, un message de commentaire C2D a le type de contenu de `application/vnd.microsoft.iothub.feedback.json` et propriétés dans son corps JSON peuvent être utilisées pour déduire l’état de remise du message d’origine :
* Clé `statusCode` dans des commentaires corps a une des valeurs suivantes : `['Success', 'Expired', 'DeliveryCountExceeded', 'Rejected', 'Purged']`.
* Clé `deviceId` dans des commentaires corps a l’ID de l’appareil cible.
* Clé `originalMessageId` dans des commentaires corps a l’ID du message C2D d’origine envoyé par le service. Cela peut servir à mettre en corrélation les commentaires pour les messages C2D.

### <a name="receive-telemetry-messages-service-client"></a>Recevoir des messages de télémétrie (client de service)


### <a name="additional-notes"></a>Remarques supplémentaires
* Les connexions AMQP peuvent être interrompues en raison d’un réseau problème ou expiration du jeton d’authentification (généré dans le code). Le client du service doit gérer ces circonstances et rétablir la connexion et les liens si nécessaire. Dans le cas d’expiration du jeton d’authentification, le client peut renouveler également de manière proactive le jeton avant son expiration afin d’éviter une baisse de la connexion.
* Dans certains cas, votre client doit être en mesure de gérer correctement les redirections de liaison. Reportez-vous à la documentation de votre client AMQP sur comment effectuer cette opération.

### <a name="receive-cloud-to-device-messages-device-and-module-client"></a>Recevoir des messages cloud-à-appareil (appareil et le module client)
Liens AMQP utilisés sur le côté de l’appareil sont les suivantes :

| Créé par | Type de lien | Chemin d’accès du lien | Description  |
|------------|-----------|-----------|-------------|
| Appareils | Lien du récepteur | `/devices/<deviceID>/messages/devicebound` | Messages C2D destinés aux appareils sont reçus sur ce lien par chaque périphérique de destination. |
| Appareils | Lien de l’expéditeur | `/messages/serviceBound/feedback` | Commentaires de messages C2D envoyées au service sur ce lien par les appareils. |
| Modules | Lien du récepteur | `/devices/<deviceID>/modules/<moduleID>/messages/devicebound` | Messages C2D destinés aux modules sont reçus sur ce lien par chaque module de destination. |
| Modules | Lien de l’expéditeur | `/messages/serviceBound/feedback` | Commentaires de messages C2D envoyées au service sur ce lien par les modules. |


## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur le protocole AMQP, consultez le [spécification du protocole AMQP v1.0](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Pour en savoir plus sur les messages IoT Hub, consultez :

* [Messages cloud-à-appareil](./iot-hub-devguide-messages-c2d.md)
* [Prise en charge des protocoles supplémentaires](iot-hub-protocol-gateway.md)
* [Prise en charge de protocole MQTT](./iot-hub-mqtt-support.md)
