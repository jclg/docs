.. masterpush documentation master file, created by
   sphinx-quickstart on Tue Dec  8 18:45:53 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Bienvenue dans la documentation de Masterpush
=============================================

.. _masterpush: https://www.masterpush.com/
.. _Curl: http://php.net/manual/fr/book.curl.php

`Masterpush`_ est un service d'envoi de masse dédié au SMS. Il permet à nos clients d'envoyer des campagnes
de marketing via SMS.

Le but de cette documentation et de permettre à un utilisateur de l'API Masterpush d'implémenter rapidement
un module d'envoi de sms sur son application.

Documentation pour PHP :

Afin d'utiliser l'API, vous devez pouvoir envoyer des requêtes HTTP à nos serveurs. Cela peut être fait via
l'extension php `Curl`_.

Installer Curl
**************

Sur ubuntu
----------

.. code-block:: bash

  $> apt-get install php5-curl
  $> service apache2 restart

Si vous utilisez php-fpm :

.. code-block:: bash

  $> service php5-fpm restart

S'authentifier
**************

Afin de vous identifier sur l'API, il vous sera demandé d'ajouter un token (clé) d'authentification à chaque requête.
Celui-çi vous sera communiqué par un de nos account Managers ou commercial, il peut être révoqué à tout moment.
Pour cela, il suffit de demander la génération d'un nouveau token.

La clé doit être ajoutée dans les headers des requêtes. Voici comment procéder avec Curl:

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();
  setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

Recupérer la liste des messages
*******************************

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
*********************

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


Envoyer un message
******************

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
