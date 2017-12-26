Créer et utiliser une Brique
============================

Premier pas pour créer une Brique
---------------------------------

Une brique peut être créée dans un plugin ou un thème WordPress.
Voici un exemple de code minimaliste pour la création d’une Brique via un plugin WordPress :

``/wp-content/plugins/mc_briques_exemple/mc_briques_exemple.php``

.. code-block:: php

    <?php
    /**
     * Plugin Name: Médias Cité : BRIQUES Exemple
     * Description: Ajoute la Brique Exemple
     * Version: 1.0
     * Author: Cogitime
     * License: GPL2
     * Licence URI: https://www.gnu.org/licenses/gpl-2.0.html
     * Text Domain: mcbriques
     * Domain Path: /languages
     *
     * @package Briques
     * @category Exemple
     * @author Cogitime
     */
    
    if(!defined('ABSPATH'))
    	exit;
    
    define('MCBRIQUES_EXEMPLE_PLUGIN_FILE', __FILE__);
    
    if(!class_exists('MCBriquesExemple')){
    	require_once(dirname(MCBRIQUES_EXEMPLE_PLUGIN_FILE).'/classes/MCBriquesExemple.php');
    	MCBriquesExemple::init();
    }
    ?>

Si la classe principale du plugin n'existe pas alors on l'inclut puis on exécute la fonction init.



Initialisation d'une brique
---------------------------


Charger une brique
------------------


Partage de fonctions entre les Briques
--------------------------------------


Options paramétrables via une interface d'administration
--------------------------------------------------------

Ajouter des options à votre Brique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ajouter des options à une autre Brique existante via votre Brique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Récupération des options d'une Brique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Override/Remplacement d'une Brique existante
--------------------------------------------

Surcharge partielle
~~~~~~~~~~~~~~~~~~~

Surcharge totale (remplacement d'une Brique)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

