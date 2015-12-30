Statistiques
============

L'api masterpush vous permet de retrouver vos statistiques en choisissant la granularitée.

Général
-------

Vous pouvez retrouver vos statistiques depuis la création de la configuration en envoyant une requête GET vers la route /statistics.
Le code ci-dessous va recupèrer les statistics du compte.

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/statistics');

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output ;

Cette route retourne un objet JSON comme celui-dessous :

Par heure
---------

Pour avoir les statistics du compte par heure, il suffit d'envoyer une requête GET vers la route /statistics/hourly comme ci-dessous:

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/statistics/hourly');

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output ;

Le retour de cette route est un objet JSON comme si dessous :

.. code-block:: json

  [
    {
      "date" => "2015-01-01 00:00:00",
      "mt" => 1,
      "operators" => [
        ["name" => "sfr", "mt" => 42], {...}
      ],
      "status" => [
        {"status" => "ok", "mt" => 42}, {...}
      ]
    }, {...}
  ]
.. code-block:: json

  [
    {
      "mt" => 1,
      "operators" => [
        ["name" => "sfr", "mt" => 42], {...}
      ],
      "status" => [
        {"status" => "ok", "mt" => 42}, {...}
      ]
    }, {...}
  ]

Par jour
--------

Pour avoir les statistics du compte par jour, il suffit d'envoyer une requête GET vers la route /statistics/daily comme ci-dessous:

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/statistics/daily');

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output ;

Le retour de cette route est un objet JSON comme si dessous :

.. code-block:: json

  [
    {
      "date" => "2015-01-01 00:00:00",
      "mt" => 1,
      "operators" => [
        ["name" => "sfr", "mt" => 42], {...}
      ],
      "status" => [
        {"status" => "ok", "mt" => 42}, {...}
      ]
    }, {...}
  ]

Par mois
--------

Pour avoir les statistics du compte par mois, il suffit d'envoyer une requête GET vers la route /statistics/monthly comme ci-dessous:

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/statistics/monthly');

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output ;

Le retour de cette route est un objet JSON comme si dessous :

.. code-block:: json

  [
    {
      "date" => "2015-01-01 00:00:00",
      "mt" => 1,
      "operators" => [
        ["name" => "sfr", "mt" => 42], {...}
      ],
      "status" => [
        {"status" => "ok", "mt" => 42}, {...}
      ]
    }, {...}
  ]

Filtrer les statistiques
------------------------

Toutes les statistiques peuvent être filtrer par date. Pour cela, il suffit d'ajouter a l'url les paramètres "from" et "to".
Ils doivents être formaté comme la date ci dessous (année-mois-jour heure:minute:seconde):

      2015-04-21 23:42:42

Le code ci-dessous donne un exemple :

.. code-block:: php

  $token = 'mytoken';
  $ch = curl_init();

  // add the token
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('x-masterpush-apikey: ' . $token));

  // set the route URL
  curl_setopt($ch, CURLOPT_URL, 'https://api.masterpush.com/v1/statistics?from='. urlencode('2015-04-21 23:42:42').'&to='.urlencode('2015-06-21 23:42:42'));

  curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
  // Execute the query
  $server_output = curl_exec($ch);

  // Close the connection
  curl_close ($ch);
  print  $server_output ;

