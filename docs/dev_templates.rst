Système de templates
====================

Le système de templates est ajouté par la brique **Template** présente dans le plugin principal.

Charger un template
-------------------

Un template est lié à une brique et doit être chargé via la brique concernée.

Chaque brique possède son dossier de templates. Ce dernier est définit lors de l'enregistrement de la brique via le second paramètre de la fonction **registerBrique** qui correspond au nom du plugin incluant la brique.

Par exemple, le dossier de templates de la brique Exemple serait ``/wp-content/plugins/mc_briques_exemple/templates/Exemple/`` avec l'enregistrement suivant :

.. code-block:: php

    MCBriques::registerBrique('Exemple', 'mc_briques_exemple', $briquesPath.'Exemple.php', 'MCBriqueExemple');

.. note::
    Voir le chapitre suivant pour surcharger/remplacer un template

Voici un exemple de code pour charger un fichier de template de la brique Exemple :

.. code-block:: php

    <?php
    // Charge la Brique "Exemple"
    $exemple = MCBriques::loadBrique('Exemple');
    
    // Affiche le template "mon-template.php"
    $exemple::loadTemplate('mon-template.php', array(), false);
    
    // Affiche le template "mon-template.php" en lui passant un un argument
    $exemple::loadTemplate('mon-template.php', array('nomVariable'=>'Valeur variable'), false);

Surcharger/Remplacer un template
-------------------------------

Via un plugin WordPress
~~~~~~~~~~~~~~~~~~~~~~~

Le système de chargement de templates utilise le filtre **mcb_brique_template** ce qui permet d'intercepter le chargement d'un template.

Voici un exemple de code pour remplacer un template via le filtre **mcb_brique_template** :

.. code-block:: php

    <?php
    class MCBriqueExemple extends MCBrique
    {
    	const BRIQUE = 'Exemple';
    
    	/**
    	 * Initialisation de la brique Exemple
    	 */
    	public static function init(){
    		add_filter('mcb_brique_template', array(__CLASS__, 'mcb_brique_template'), 10, 3);
    	}
    	
    	public static function mcb_brique_template($templateFile, $template, $brique){
    		if($brique == 'AutreBrique' && $template == 'template.php'){
    			$templateFile = static::getTemplatePath('AutreBrique/template.php');
    		}
    		return $templateFile;
    	}
    }

.. note::
    *La Brique « AutreBrique » n’existe pas par défaut, il s’agit simplement d’un exemple.*
    
    Ce code aura pour effet d'intercepter le chargement du template "template.php" de la brique "AutreBrique" afin de le remplacer par le template "AutreBrique/template.php" de la brique "Exemple".
    
    Le template chargé au final sera donc le suivant : ``/wp-content/plugins/mc_briques_exemple/templates/Exemple/AutreBrique/template.php``

Lors de l'affichage d'un template, une action WordPress est exécutée avant ( **mcb_brique_template_before** ) et après ( **mcb_brique_template_after** ) l'affichage. Vous pouvez donc utiliser ces actions pour afficher d'autres templates avant et/ou après un template spécifique (sans pour autant remplacer le template concerné).

Voici un exemple de code :

.. code-block:: php

    <?php
    class MCBriqueExemple extends MCBrique
    {
    	const BRIQUE = 'Exemple';
    
    	/**
    	 * Initialisation de la brique Exemple
    	 */
    	public static function init(){
    		add_action('mcb_brique_template_after', array(__CLASS__, 'mcb_brique_template_after'), 10, 3);
    	}
    	
    	public static function mcb_brique_template_after($brique, $template, $args){
    		if($brique == 'AutreBrique' && $template == 'template.php'){
    			static::loadTemplate('AutreBrique/after-template.php', $args, false);
	    	}
    	}
    }

.. note::
    *La Brique « AutreBrique » n’existe pas par défaut, il s’agit simplement d’un exemple.*
    
    Le template "AutreBrique/after-template.php" de la brique "Exemple" sera affiché après le chargement du template "template.php" de la brique "AutreBrique".

Via un thème WordPress
~~~~~~~~~~~~~~~~~~~~~~

Il est possible de remplacer un fichier de template très facilement via votre thème WordPress.

Pour cela il est nécessaire de créer un dossier ``mc_briques`` à la racine de votre thème afin d'y insérer tous les templates que vous souhaitez modifier.

Par exemple, pour modifier le template ``/wp-content/plugins/mc_briques_exemple/templates/Exemple/mon-template.php``, il faudra créer le fichier suivant : ``/wp-content/themes/*mon-theme*/mc_briques/Exemple/mon-template.php``

Si le template est remplacé par un plugin (voir le chapitre précédent) alors il est nécessaire de créer le fichier correspondant au nouveau template définit par le plugin. Dans notre exemple, le nouveau template à créer dans le thème serait donc le suivant : ``/wp-content/themes/*mon-theme*/mc_briques/Exemple/AutreBrique/template.php``

Ajouter un template de page
---------------------------