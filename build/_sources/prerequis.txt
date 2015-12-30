Prérequis
=========

Installer Curl
--------------

Sur ubuntu
~~~~~~~~~~

.. code-block:: bash

  $> apt-get install php5-curl
  $> service apache2 restart

Si vous utilisez php-fpm :

.. code-block:: bash

  $> service php5-fpm restart

S'authentifier
--------------

Afin de vous identifier sur l'API, il vous sera demandé d'ajouter un token (clé) d'authentification à chaque requête.
Celui-çi vous sera communiqué par un de nos account Managers ou commercial, il peut être révoqué à tout moment.
Pour cela, il suffit de demander la génération d'un nouveau token.

La clé doit être ajoutée dans les headers des requêtes. Voici comment procéder avec Curl:

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();
  setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

