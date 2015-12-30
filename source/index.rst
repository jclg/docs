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

.. _Avant-propos:

.. toctree::
  :maxdepth: 2
  :caption: Presentation

  prerequis


.. _developer-docs:

.. toctree::
  :maxdepth: 2
  :caption: Developer Documentation

  messages
  statistics
