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

Interface des champs personnalisés
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


.. image:: /img/customfields-1.jpg

1. Pour ajouter un champs, sélectionnez le type de champ puis cliquez sur "Ajouter le champ"
2. L'icône engrenage permet d'ouvrir les options du champ
3. Les flèches permettent de changer l'ordre des champs
4. La croix permet de supprimer le champ (les valeurs stockées ne seront pas supprimées)

.. warning::
    Une fois les champs personnalisés configurés, il est nécessaire d'enregistrer la configuration en cliquant sur "Enregistrer".

Onglet "Annonces"
=================

Paramètres
----------

Page d'archives
~~~~~~~~~~~~~~~

Voir le chapitre :ref:`configuration-page`. Cette option est requise pour le bon fonctionnement de la liste des annonces en mode contenu.

Format de date
~~~~~~~~~~~~~~

Il s'agit d'un format de date compatible avec la fonction date de PHP : http://php.net/manual/fr/function.date.php

Par exemple, le format ``d/m/Y H:i`` permet d'obtenir une date dont la structure est la suivante : ``25/12/2017 17:00``

Activer les prix
~~~~~~~~~~~~~~~~

Vous avez la possibilité d'utiliser les annonces sans activer les prix.

Prix minimum
~~~~~~~~~~~~

Prix minimum autorisé (nécessite l'activation du prix).

Prix maximum
~~~~~~~~~~~~

Prix maximum autorisé (nécessite l'activation du prix).

Autoriser gratuit
~~~~~~~~~~~~~~~~~

Vous pouvez autoriser les annonces gratuites même si vous avez définit un prix minimun supérieur à 0€.

Affichage par page
~~~~~~~~~~~~~~~~~~

Nombre d'annonces à afficher par page.

Nombre de photos autorisées
~~~~~~~~~~~~~~~~~~~~~~~~~~

Renseignez ce champ pour restreindre le nombre de photos disponibles lors de l'enregistrement d'une annonce.

Poids max par photo (ko)
~~~~~~~~~~~~~~~~~~~~~~~~

Limitez le poids max par photo.

Autoriser la modification
~~~~~~~~~~~~~~~~~~~~~~~~~

Cette option permet d'autoriser ou non la modification d'une annonce par son auteur.

Nombre de tags max
~~~~~~~~~~~~~~~~~~

Pour activer les tags sur les annonces vous devez définir le nombre maximum de tags autorisés par annonce.

Frais de port max
~~~~~~~~~~~~~~~~~

Vous avez la possibilité de limiter les frais de port max.

Onglet "Commandes"
==================

Paramètres
----------

Mail alerte nouvelles commandes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adresse mail à notifier lors de nouvelles commandes. Laissez vide pour ne pas déclencher les notifications. Cette option nécessite l'installation du module Mail ``mc_briques_mail``.

Page des CGV
~~~~~~~~~~~~

Voir le chapitre :ref:`configuration-page`. Cette option permet d'ajouter des CGV dans le process de commande, l'utilisateur sera obligé de cocher une case pour valider les CGV avant de pouvoir finaliser sa commande.

Commission sur les reversements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Configuration du pourcentage de commission à retenir sur les commandes. Cette option est ajoutée par le module Reversement ``mc_briques_reversement``.

Onglet "Paiements"
==================

Vous trouverez dans cet onglet les différents moyens de paiements disponibles. Par défaut seul le paiement par chèque est présent. Le paiement par virement est ajouté par le module Virement ``mc_briques_virement`` et le paiement par Paypal est ajouté par le module Paypal ``mc_briques_paypal``.

Pour configurer un moyen de paiement il suffit de cliquer sur le nom du paiement afin d'accéder à son interface de configuration.

Configuration récurrente à tous les paiements
---------------------------------------------

Voici les configurations disponibles sur tous les moyens de paiement :

:Actif: **Oui** ou **Non** afin d'activer ou non le moyen de paiement
:Intitulé: Vous avez la possibilité de renommer le moyen de paiement
:Description: Ce texte est affiché dans le process de commande lors de la sélection du paiement ainsi que dans les récapitulatif

Configuration du paiement par chèque
------------------------------------

Le paiement par chèque possède un paramètre **Instructions** qui est affiché sur la commande afin d'indiquer les diverses instructions de paiement.

Il est possible d'insérer certaines valeurs liées à la commande via les codes suivants :

:{$commande.total}: Montant total de la commande
:{$commande.reference}: Référence de la commande

Voici un exemple d'instructions pour le paiement par chèque :

.. code-block:: html

    Afin de régler votre commande, veuillez nous envoyer un chèque avec les informations suivantes :
    <ul>
     	<li><strong>Montant du règlement : </strong>{$commande.total} €</li>
     	<li><strong>A l'ordre de :</strong> ....</li>
     	<li><strong>Envoyer à l'adresse suivante :</strong> ....</li>
     	<li><strong>N'oubliez pas d'indiquer votre référence de commande :</strong> {$commande.reference}</li>
    </ul>

.. note::
    Afin de copier coller cet exemple, il est nécessaire de passer la rédaction de l'instruction en mode "Texte" (via le petit onglet en haut à droite de l'éditeur de texte). Vous pouvez repasser en mode "Visuel" ensuite.

Configuration du paiement par virement
--------------------------------------

La configuration du paiement par virement est très similaire à celle du paiement par chèque, vous pouvez donc vous référer au chapitre ci-dessus.

Voici un exemple d'instructions pour le paiement par virement :

.. code-block:: html

    Afin de régler votre commande, veuillez effectuer un virement de {$commande.total} € aux coordonnées bancaires suivantes :
    <strong>IBAN :</strong> .... ..... ..... ........... ..
    
    <strong>N'oubliez pas d'indiquer votre référence de commande :</strong> {$commande.reference}

Configuration du paiement par Paypal
------------------------------------

L'intégration du paiement utilise la fonctionnalité IPN de Paypal (Notification instantanée de paiement).

Les paramètres disponibles sont les suivants :

:E-mail Paypal: Adresse mail du compte Paypal devant recevoir les paiements
:Mode test (Sandbox): **ATTENTION** Cette option permet d'utiliser les serveurs de **test** de Paypal ! Les paiements effectués en mode test ne sont pas réellement effectués !
:URL de notification: Il s'agit de l'adresse à renseigner sur la configuration IPN de votre compte Paypal (voir ci-dessous)

Configuration IPN Paypal
~~~~~~~~~~~~~~~~~~~~~~~~

Afin d'activer l'option IPN sur le compte Paypal, suivez les étapes suivantes :

1. Connectez vous à votre compte sur le site officiel Paypal (vous devez avoir un compte Business)
2. Accédez au menu "Préférences" puis "Paramètres du compte"
3. Accédez au menu "Mes ventes (Réception de paiements, livraison, etc.)"
4. Dans la section "Obtenir des paiements et gérer mes risques", sur la ligne du paramètre "Notifications instantanées de paiement", cliquez sur le lien "Mettre à jour"
5. Cliquez sur "Choisir les paramètres IPN" (si vous avez déjà configuré votre IPN vous n'aurez pas cette étape)
6. Renseignez l'URL de notification, choisissez l'option "Recevoir les messages IPN (activé)" puis cliquez sur "Enregistrer"


Onglet "Commentaires"
=====================

Paramètres
----------

Mail alerte nouveau commentaire
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adresse mail à notifier lors d'un nouveau commentaire. Laissez vide pour ne pas déclencher les notifications. Cette option nécessite l'installation du module Mail ``mc_briques_mail``.

Onglet "Mails"
==============

Paramètres
----------

Mail en HTML
~~~~~~~~~~~~

Les templates de mail sont disponibles au format HTML et Texte. Vous pouvez choisir le format à envoyer en cochant ou non cette option.

Adresse mail d'expédition
~~~~~~~~~~~~~~~~~~~~~~~~~

Adresse mail utilisée pour l'expédition des mails envoyés par les Briques.

Nom d'expédition
~~~~~~~~~~~~~~~~

Nom lié à l'adresse d'expédition.

En-tête du mail HTML
~~~~~~~~~~~~~~~~~~~~

Code HTML inséré dans l'en-tête des mails envoyés par les Briques.

.. note::
    **Balises HTML autorisées par défaut :** p, span, a, img, ul, ol, li, del, em, string, table, thead, tbody, tfoot, tr, th, td

En-tête du mail Texte
~~~~~~~~~~~~~~~~~~~~~

Version Texte de l'en-tête.

Signature du mail HTML
~~~~~~~~~~~~~~~~~~~~~~

Code HTML inséré dans la signature des mails envoyés par les Briques.

.. note::
    **Balises HTML autorisées par défaut :** p, span, a, img, ul, ol, li, del, em, string, table, thead, tbody, tfoot, tr, th, td

Signature du mail Texte
~~~~~~~~~~~~~~~~~~~~~~~

Version Texte de la signature.

Gestion des utilisateurs
========================

L'accès à la gestion des utilisateurs nécessite un compte administrateur. Il est cependant possible d'ouvrir l'accès à un autre utilisateur en lui attribuant le droit nécessaire.

La gestion des utilisateurs permet d'accéder aux informations de base des utilisateurs ainsi qu'à leurs droits d'accès.

Les droits d'accès sont ajoutés par les diverses briques installées sur votre site. Dans la majorité des cas il s'agit de droits d'administration.

Il est ainsi facile de déléguer la gestion des utilisateurs sans lui donner les accès administrateur.