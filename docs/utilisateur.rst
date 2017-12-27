Introduction
============

Cette documentation a pour but de détailler les différentes options de configuration des principales Briques du système Médias Cité 4Me.

.. _configuration-page:

Configuration d'une page
------------------------

Parmi les différentes options disponibles, certaines font référence à la sélection d'une page WordPress.

La page à sélectionner doit donc correspondre au paramètre concerné.

.. warning::
    Dans certains cas, le simple fait de sélectionner la page ne suffit pas.
    Il est alors nécessaire d'insérer un shortcode dans la page (ce shortcode est alors indiqué sous la sélection).

Onglet "Général"
================

Mode d'intégration
------------------

Le mode d'intégration est actuellement restreint au mode "Contenu". Ce mode permet l'insertion des templates à la place du contenu standard d'une page, c'est pour cela qu'il est nécessaire de configurer certaines pages comme la liste des annonces.

Intégrations Bootstrap, FontAwesome, Fancybox
---------------------------------------------

Certain thèmes utilisent déjà Bootstrap, FontAwesome et/ou Fancybox. Vous avez donc la possibilité d'activer/désactiver l'intégration de ces librairies afin de ne pas les charger en double.

API Google Maps
---------------

.. note::
    Le paramétrage des clés API Google Maps est ajouté par le module Géolocalisation ``mc_briques_geoloc``. Ce module permet d'ajouter des champs personnalisés de type géolocalisation.

Deux clés sont paramétrables afin de vous permettre de gérer au mieux les restrictions d'accès proposées par Google.

En effet, la clé "Google Maps Javascript" est utilisée par le navigateur internet du visiteur afin de lui proposer une liste des villes via le champ de recherche, il est donc possible de restreindre cette clé à un référent HTTP (adresse du site).
La clé "Google Maps Geocode" est utilisée par le serveur qui héberge votre site afin de valider la géolocalisation saisie par le visiteur. La seule restriction qu'il est possible de configurer pour cette clé est une restriction sur l'adresse IP (adresse de votre serveur web).

`Accéder à la configuration des clés API Google <https://console.developers.google.com/>`_

Onglet "Administration"
=======================

Paramètres
----------

Page de l'administration
~~~~~~~~~~~~~~~~~~~~~~~~

Voir le chapitre :ref:`configuration-page`.

Onglet "Utilisateurs"
=====================

Paramètres
----------

Page de connexion, déconnexion, inscription, passe oublié, reset passe, profil privé, profil public
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Voir le chapitre :ref:`configuration-page`.

Poids max upload avatar (ko)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Poids maximum autorisé pour l'avatar d'un utilisateur.

Commentaires et notes
---------------------

.. note::
    Les options de commentaires et de notes sont ajoutées par le module Commentaires ``mc_briques_commentaire``.

Les commentaires et les notes sont liés mais vous avez la possibilité d'activer l'un ou l'autre indépendamment, ou les deux.

Champs personnalisés
--------------------

Les champs personnalisés vous permettent d'ajouter des informations paramétrables sur un compte utilisateur.

Un champ possède au minimum les informations suivantes :

:Type de champ: Deux types existent par défaut, Texte et Sélection. Il est possible d'avoir d'autres types de champ via des modules (comme le champ Géolocalisation ajouté par le module du même nom).
:Intitulé du champ: L'intitulé (label) est affiché sur le site.
:Nom du champ: Le nom du champ est la clé de stockage. Si vous modifiez le nom d'un champ alors vous perdrez les valeurs saisies sur le précédent nom (à moins de renommer le champs avec son ancien nom).
:Champ requis: Cette option permet d'indiquer si le champ est requis lors de l'envoi du formulaire.
:Champ privé: Cette option permet au champ de ne pas être affiché publiquement.

Onglet "Annonces"
=================


Onglet "Commandes"
==================


Onglet "Paiements"
==================


Onglet "Commentaires"
=====================


Onglet "Mails"
==============


