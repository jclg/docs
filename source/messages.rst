Envoyer/Retrouver des messages
==============================

Recupérer la liste des messages
-------------------------------

Pour avoir la liste des messages, il vous suffit d'envoyer une requête GET vers la route /messages

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/messages');

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output;

Le code ci-dessus devrait retourner un object JSON correspondant au template ci-dessous :

.. code-block:: json

  [
    {
      "id": 0,
      "phone": "+33xxxxxxxxx",
      "content": "La vie l'univers et tout le reste",
      "status": "ok",
      "date": "2015-04-05 23:42:42"
    }
  ]

Si aucun message n'a été envoyé via cette configuration, le tableau sera vide.

Filtrer les résultats
~~~~~~~~~~~~~~~~~~~~~

Il est possible de filtrer les résultats d'une requête. Pour cela, il faut ajouter les filtres dans
l'url.

Il y a six filtres disponibles sur cette ressource :

- phoneNumber : Limite la recherche au numéro
- status : Limite la recherche statut des messages, il peut être

  - "ok"
  - "ko"
  - "error"
  - "waiting"
  - "queued"
  - "toQueued"
  - "processing"

- from : Limite la recherche aux messages envoyés après cette date, le format de date est "Y-m-d H:i:s"
- to : Limite la recherche aux messages envoyés jusqu'à cette date, le format de date est "Y-m-d H:i:s"
- limit: Limite le nombre de resultats (100 max)
- offset : Saute les x premiers resultats.

Voici un exemple pour avoir les messages envoyés au +33xxxxxxxxx qui sont ko:


.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/messages?phoneNumber=' . urlencode('+33xxxxxxxxx') . '&status=ko');

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output ;

Retrouver un message
--------------------

Vous pouvez retrouver un message a partir de son ID.
Pour faire ça, il suffit d'envoyer une requête GET vers la route /messages/{ID} où {ID} doit être remplacé par l'ID du message en question.
Le code ci-dessous retournera les informations du message a l'id  '42sh' (il n'existe pas).

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/messages/42sh');

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output ;

Le code ci-dessus devrait retourner un object JSON correspondant au template ci-dessous :

.. code-block:: json

    {
      "id": 0,
      "phone": "+33xxxxxxxxx",
      "content": "La vie l'univers et tout le reste",
      "status": "ok",
      "date": "2015-04-05 23:42:42"
    }


Envoyer un message
------------------

Pour envoyer un message via notre API, il suffit d'envoyer une requête POST vers la route /messages
Cette route demande un object JSON dans le body contenant au moins les champs:

- phoneNumber
- content

Le champ oadc est optionnel et changera le sender affiché sur le téléphone du destinataire.
Le code ci-dessous va envoyé un sms au numéro +336xxxxxxxx avec le l'OADC masterpush.

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/messages');
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($ch, CURLOPT_POST, true);
  curl_setopt($ch, CURLOPT_POSTFIELDS,     '{"phoneNumber": "+336xxxxxxx", "content": "La vie l\'univers et tout le reste", "oadc": "masterpush"}' );

  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output;

La route /messages retourne un object json représentant le message, il contient l'id du message, le numéro du destinataire, le status, et la date d'envoi.

Exemple de retour avec un code http 201:

.. code-block:: json

    {
      "id": 0,
      "phone": "+33xxxxxxxxx",
      "content": "La vie l'univers et tout le reste",
      "status": "waiting",
      "date": "2015-04-05 23:42:42"
    }


Plusieurs retours sont possibles:

+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Cas                               | Code http de la réponse   | Body de la réponse (exemple)                                                                                   |
+===================================+===========================+================================================================================================================+
| Envoi accepté                     | 201                       | {"id":0,"phoneNumber":"+336xxxxxxx","content":"Contenu","status":"waiting","date":"2017-03-16T08:55:48+0100"}  |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Requête/json malformée            | 400                       | {"error":"Messages JSON object not found or not valid"}                                                        |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Champ manquant ou non attendu     | 400                       | {"errors":{"test":"This field was not expected."}}                                                             |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Non autorisé pour l'international | 403                       | {"error":"Sending messages to international phone numbers is not allowed"}                                     |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Cas d'erreur spécifique           | 4XX ou 5XX                | {"error":"Message d'erreur spécifique"}                                                                        |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+

Ce tableau n'est pas exhaustif.
Dans certains cas spécifiques, un code de retour http 4XX ou 5XX peut-être retourné avec un body json {"error":"Message d'erreur"}


Envoi multiple
--------------

Vous pouvez envoyer plusieur message en une requête. Pour cela il suffit d'envoyer une requête POST vers la route /messages/multiple.
Cette route prend pour argument une array de message. L'exemple ci-dessous envera deux messages.

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/messages/multiple');
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  curl_setopt($ch, CURLOPT_POST, true);
  curl_setopt($ch, CURLOPT_POSTFIELDS,     '[
    {"phoneNumber": "+336xxxxxxx", "content": "La vie l\'univers et tout le reste", "oadc": "masterpush"},
    {"phoneNumber": "+336xxxxxxx", "content": "un autre message", "oadc": "masterpush"}
    ]' );

  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output;


La route /messages/multiple retourne un object json représentant une array de messages.

.. code-block:: json

    [{
      "id": 0,
      "phone": "+33xxxxxxxxx",
      "content": "La vie l'univers et tout le reste",
      "status": "waiting",
      "date": "2015-04-05 23:42:42"
    },{
      "id": 42,
      "phone": "+33xxxxxxxxx",
      "content": "un autre message",
      "status": "waiting",
      "date": "2015-04-05 23:42:42"
    }]

Comme pour l'envoi simple, plusieurs retours sont possibles:

+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Cas                               | Code http de la réponse   | Body de la réponse (exemple)                                                                                   |
+===================================+===========================+================================================================================================================+
| Envoi multiple accepté            | 201                       | Tableau json représentant la liste des messages                                                                |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Requête/json malformée            | 400                       | {"error":"Message collection not found"}                                                                       |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Champ manquant ou non attendu     | 400                       | {"errors":[{"test":"mavaleur This field was not expected."}]}                                                  |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+
| Cas d'erreur spécifique           | 4XX ou 5XX                | {"error":"Message d'erreur spécifique"}                                                                        |
+-----------------------------------+---------------------------+----------------------------------------------------------------------------------------------------------------+

Il est important de noter que même si l'envoi multiple est accepté, certains messages peuvent ne pas être envoyés pour diverses raisons.
Il est donc conseillé de vérifier le champ "status" de chaque message dans la liste retournée.

Statuts des messages
--------------------

Le champ "status" représente l'état du message.

+----------------------------+---------------------------------------------------------------------------------------------+
| Valeur du champ "status"   | Signification                                                                               |
+============================+=============================================================================================+
| waiting                    | Le message est en cours d'envoi (en attente de l'accusé de réception par l'opérateur)       |
+----------------------------+---------------------------------------------------------------------------------------------+
| ok                         | Le message a été remis au destinataire                                                      |
+----------------------------+---------------------------------------------------------------------------------------------+
| ko                         | Le message n'a pas été remis au destinataire (échec)                                        |
+----------------------------+---------------------------------------------------------------------------------------------+
| error                      | Une erreur est survenue lors du traitement du message                                       |
+----------------------------+---------------------------------------------------------------------------------------------+
| queued                     | Le message est dans la file d'attente                                                       |
+----------------------------+---------------------------------------------------------------------------------------------+
| toQueued                   | Le message va être mis dans la file d'attente                                               |
+----------------------------+---------------------------------------------------------------------------------------------+
| processing                 | Le message est en cours de traitement dans la file d'attente                                |
+----------------------------+---------------------------------------------------------------------------------------------+
