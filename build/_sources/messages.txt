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

.. code-block:: json

    {
      "id": 0,
      "phone": "+33xxxxxxxxx",
      "content": "La vie l'univers et tout le reste",
      "status": "waiting",
      "date": "2015-04-05 23:42:42"
    }

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
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/messages');
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


