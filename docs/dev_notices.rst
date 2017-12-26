Système de notices
==================

Le système de notices est ajouté par la brique **Notice** présente dans le plugin principal.

Les notices sont temporairement stockées dans la session de l'utilisateur avant d'être affichées (vous pouvons donc ajouter des notices puis réaliser une redirection sans perdre vos notices et sans ajouter de paramètres supplémentaires dans la redirection).

Utilisation classique
---------------------

Les notices sont utilisables depuis n'importe quelle brique. Voici un exemple pour ajouter et afficher une notice :

.. code-block:: php

    <?php
    // Charge la Brique "Exemple"
    $exemple = MCBriques::loadBrique('Exemple');
    
    // Ajouter une notice
    $exemple::addNotice('Ma notice de type warning', 'warning');
    
    // Affiche les notices
    $exemple::displayNotices(null, false);
    
    // Affiche directement une notice sans passer par l'ajout
    $exemple::displayNotice('Texte de la notice ...', 'warning', false);


Utilisation avec WP_Error
-------------------------

La fonction **addNotice** est compatible avec l'objet `WP_Error <https://codex.wordpress.org/Class_Reference/WP_Error>`_, voici un exemple d'utilisation :

.. code-block:: php

    <?php
    $wp_error = new WP_Error();
    $wp_error->add('danger', "Message d'erreur 1");
    $wp_error->add('code_custom', "Message d'erreur 2");
    
    $exemple = MCBriques::loadBrique('Exemple');
    $exemple::addNotice($wp_error, 'warning');
    $exemple::displayNotices(null, false);

.. note::
    Dans cet exemple, le message d'erreur numéro 1 sera de type "danger" alors que le message d'erreur numéro 2 sera de type "warning" car son code ne correspond pas à un type prédéfinit (il utilisera donc le type indiqué lors du **addNotice**)