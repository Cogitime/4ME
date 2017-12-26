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

.. note::
    Si la classe principale du plugin n'existe pas alors on l'inclut puis on exécute la fonction init.

``/wp-content/plugins/mc_briques_exemple/classes/MCBriquesExemple.php``

.. code-block:: php

    <?php
    /**
     * Classe principale du plugin Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Classe principale du plugin Exemple
     *
     * @class 		MCBriquesExemple
     * @category	Class
     * @package 	Briques\Classes
     */
    class MCBriquesExemple
    {
    	/**
    	 * Initialisation du plugin
    	 */
    	public static function init(){
    		add_action('plugins_loaded', array(__CLASS__, 'action_pluginsLoaded'));
    	}
    	
    	/**
    	 * Action WP plugins_loaded
    	 */
    	public static function action_pluginsLoaded(){
    		if(class_exists('MCBriques')){
    			$briquesPath = dirname(__FILE__).'/briques/';
    			
    			MCBriques::registerBrique('Exemple', 'mc_briques_exemple', $briquesPath.'Exemple.php', 'MCBriqueExemple');
    		}
    	}
    }

.. note::
    La fonction init est exécutée au chargement du plugin.
    
    L'enregistrement de la brique est réalisé par la fonction **MCBriques::registerBrique** (voir la documentation du code) et doit être effectué après le chargement des plugins, via l'action WordPress `plugins_loaded <https://codex.wordpress.org/Plugin_API/Action_Reference/plugins_loaded>`_.
    
    Il est possible de re-enregistrer une Brique existante afin de la remplacer. Voir le chapitre dédié : 
    :ref:`override-brique`.

``/wp-content/plugins/mc_briques_exemple/classes/briques/Exemple.php``

.. code-block:: php

    <?php
    /**
     * Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Brique Exemple
     *
     * @class 		MCBriqueExemple
     * @category	Class
     * @package 	Briques\Classes\Brique
     */
    class MCBriqueExemple extends MCBrique
    {
    	const BRIQUE = 'Exemple';
    }

.. note::
    La constante de classe **BRIQUE** est nécessaire et doit correspondre à l'intitulé que vous avez donné à votre Brique lors de son enregistrement.

Initialisation d'une brique
---------------------------

Un fichier d'initialisation tente d'être chargé pour chaque Brique lors de l'exécution de l'action WordPress init (avec la priorité 10).

Ce fichier est **optionnel**. Il doit être placé au même emplacement que le fichier de la Brique et s'intituler de la même façon avec le suffixe **_init**. Par exemple, voici un fichier d'initialisation pour la Brique Exemple :

``/wp-content/plugins/mc_briques_exemple/classes/briques/Exemple_init.php``

.. code-block:: php

    <?php
    /**
     * Initialisation de la Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;

.. note::
    Vous pouvez ensuite ajouter tout ce que vous auriez ajouté en temps normal lors de l'exécution de l'action init de WordPress.
    
    Par exemple les fonctions suivantes : `add_action <https://developer.wordpress.org/reference/functions/add_action/>`_, `add_filter <https://developer.wordpress.org/reference/functions/add_filter/>`_, `register_post_type <https://developer.wordpress.org/reference/functions/register_post_type/>`_ ...
    
    Voir le chapitre suivant pour définir un callback vers votre Briques (pour les fonctions add_action et add_filter par exemple)

Charger une brique
------------------

Le code suivant permet de charger la Brique Exemple :

.. code-block:: php

    <?php
    $exemple = MCBriques::loadBrique('Exemple');

La fonction **loadBrique** permet de récupérer le nom de classe de la Brique souhaitée (en chaine de caractères) ou false (bool) si la Brique n'existe pas. Depuis Php 5.3 il est possible d'utiliser ce nom de classe pour appeler directement des fonctions de cette classe.

Voici divers exemples d'utilisation :

.. code-block:: php

    <?php
    $exemple = MCBriques::loadBrique('Exemple');
    
    // Charge la Brique "AutreBrique" (similaire à MCBriques::loadBrique)
    $autreBrique = $exemple::loadBrique('AutreBrique');
    
    // Affiche le template "mon-template.php"
    $exemple::loadTemplate('mon-template.php', array(), false);
    
    // Affiche une notice de type warning
    $exemple::displayNotice('Texte de la notice ...', 'warning', false);
    
    // Enregistrement de la fonction "fonction_a_executer_dans_la_brique_exemple" sur l'action WordPress "action_wordpress"
    add_action('action_wordpress', array($exemple, 'fonction_a_executer_dans_la_brique_exemple'));

.. note::
    *La Brique "AutreBrique", l'action WordPress "action_wordpress" et la fonction "fonction_a_executer_dans_la_brique_exemple" n'existent pas par défaut, il s'agit simplement d'un exemple.*

Partage de fonctions entre les Briques
--------------------------------------

Le système de Briques permet de créer des fonctions accessibles depuis n'importe quelle Brique.
Par exemple, les Briques du système d'origine incluent des fonctionnalités de **Notice**, un système de **Template** ainsi que des fonctions pratiques (**Tools**) qui sont accessibles depuis toutes les Briques (la brique Exemple permet donc d'utiliser ces fonctions).

Le code suivant est un exemple de fonctions partagées dans la Brique Exemple :

.. code-block:: php

    <?php
    class MCBriqueExemple extends MCBrique
    {
    	const BRIQUE = 'Exemple';
    	
    	/**
    	 * Initialisation de la brique Exemple
    	 */
    	public static function init(){
    		self::mapBriqueCall('exempleGetCallBrique');
    		self::mapBriqueCall('exempleAddition');
    		self::mapBriqueCall('exempleMultiplication', 'briqueMultiplication');
    	}
    	
    	public static function exempleGetCallBrique($brique){
    		return $brique;
    	}
    	
    	public static function exempleAddition($brique, $a, $b){
    		return $a+$b;
    	}
    	
    	public static function briqueMultiplication($brique, $a, $b){
    		return $a*$b;
    	}
    }

La fonction **init** d'une Brique est appelée lors du premier chargement de la Brique via la fonction **loadBrique**, ce qui permet de déclencher l'enregistrement des fonctions partagées (il est alors nécessaire de charger la Brique avant de pouvoir utiliser ses fonctions partagées).

Une fonction partagée doit être enregistrée via l'appel de **mapBriqueCall** en indiquant le nom de la fonction partagée en premier paramètre ainsi que le nom de la fonction réellement présente dans la Brique en second paramètre (si ce dernier n'est pas définit alors le premier paramètre sera utilisé).

Dans l'exemple ci-dessus on retrouve donc 3 fonctions partagées : **exempleGetCallBrique**, **exempleAddition** et **exempleMultiplication**.

**exempleMultiplication** est lié à la fonction **briqueMultiplication** présente dans la Brique.

Vous êtes libre d'avoir autant de paramètres que vous le souhaitez sur vos fonctions partagées. La seule restriction étant le premier paramètre qui correspond à la Brique appelante.

Voici un exemple d'utilisation :

.. code-block:: php

    <?php
    if(MCBriques::loadBrique('Exemple')){
    	$autreBrique = MCBriques::loadBrique('AutreBrique');
    	
    	echo $autreBrique::exempleGetCallBrique(); // AutreBrique
    	
    	echo $autreBrique::exempleAddition(5, 2); // 7
    	
    	echo $autreBrique::exempleMultiplication(5, 2); // 10
    }

.. note::
    *La Brique "AutreBrique" n'existe pas par défaut, il s'agit simplement d'un exemple.*

Options paramétrables via une interface d'administration
--------------------------------------------------------

Le système de Brique vous permet d'ajouter facilement des options paramétrables via une interface d'administration grâce à la Brique **Settings**.

L'enregistrement des options est basé sur le système de settings WordPress : `Creating Options Pages <https://codex.wordpress.org/Creating_Options_Pages>`_

Ajouter des options à votre Brique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Exemple de code complet pour ajouter un onglet d'administration dans une Brique :

``/wp-content/plugins/mc_briques_exemple/classes/briques/Exemple_init.php``

.. code-block:: php

    <?php
    /**
     * Initialisation de la Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    // Charge la Brique Settings
    $settings = MCBriques::loadBrique('Settings');
    // Charge la Brique Exemple
    $exemple = MCBriques::loadBrique('Exemple');
    
    // Si on est dans l'administration WordPress et que l'utilisateur a le droit d'accéder aux options (capability manage_options par défaut)
    if(is_admin() && $settings && current_user_can($settings::SETTINGS_CAPABILITY)){
    	// Enregistrement de l'onglet d'administration
    	$exemple::addSettingsTab('admin-exemple', 'Admin Exemple');
    }

``/wp-content/plugins/mc_briques_exemple/classes/briques/Exemple.php``

.. code-block:: php

    <?php
    /**
     * Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Brique Exemple
     *
     * @class 		MCBriqueExemple
     * @category	Class
     * @package 	Briques\Classes\Brique
     */
    class MCBriqueExemple extends MCBrique
    {
    	const BRIQUE = 'Exemple';
    	// Nom unique pour le stockage des options de la Brique (Important car utilisé par les fonction getOption et getOptions)
    	const OPTIONS = 'mcb_admin-exemple_options';
    	
    	/**
    	 * Surcharge de MCBriqueSettings::initSettingsTab
    	 *
    	 * @see MCBriqueSettings::initSettingsTab()
    	 */
    	public static function initSettingsTab($tab){
    		switch($tab){
    			case 'admin-exemple':
    				register_setting('mcb_settings_tab_admin-exemple', static::OPTIONS, array(__CLASS__, 'sanitize_options'));
    			break;
    		}
    	}
    	
    	/**
    	 * Surcharge de MCBrique::sanitize_options
    	 *
    	 * @see MCBrique::sanitize_options()
    	 */
    	public static function sanitize_options($options, $cleanOptions=array()){
    		if(!is_array($options))
    			$options = array();
    		
    		if(!is_array($cleanOptions))
    			$cleanOptions = array();
    		
    		$cleanOptions['exemple'] = (isset($options['exemple']) && $options['exemple'] == 1)? 1 : 0;
    		
    		return parent::sanitize_options($options, $cleanOptions);
    	}
    }

``/wp-content/plugins/mc_briques_exemple/templates/Exemple/Settings/tab-admin-exemple.php``

.. code-block:: php

    <?php
    /**
     * Onglet admin Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    $exemple = MCBriques::loadBrique('Exemple');
    
    $exempleOptions = $exemple::getOptions();
    
    settings_fields('mcb_settings_tab_'.$currentTab['tab']);
    do_settings_sections('mcb_settings_tab_'.$currentTab['tab']);
    ?>
    <h2>Options Exemple</h2>
    <table class="form-table">
    	<tbody>
    		<tr>
    			<th>Exemple</th>
    			<td><input type="checkbox" name="<?php echo esc_attr($exemple::OPTIONS); ?>[exemple]" value="1"<?php if(!empty($exempleOptions['exemple'])){ ?> checked="checked"<?php } ?> /></td>
    		</tr>
    	</tbody>
    </table>
    <?php
    do_action('mcb_settings_display_tab', $currentTab);
    do_action('mcb_settings_display_tab_'.$currentTab['tab'], $currentTab);
    
    submit_button('Enregistrer');

.. note::
    Il est important de garder les fonctions **settings_fields**, **do_settings_sections** et **do_action** afin d'avoir le fonctionnement par défaut du système.
    Concernant la structure HTML vous êtes libre tant que les noms des inputs correspondent.

Ajouter des options à une autre Brique existante via votre Brique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Si l'interface d'administration d'une Brique a été bien construite (comme dans l'exemple précédent avec l'utilisation de do_action dans le template) alors vous pourrez y ajouter vos options.

Pour cela vous devez enregistrer au moins une action (pour l'affichage du formulaire) et un filtre (pour le traitement du formulaire).

Voici un exemple de code à insérer dans le fichier d'initialisation de votre Brique :

.. code-block:: php

    <?php
    $settings = MCBriques::loadBrique('Settings');
    $exemple = MCBriques::loadBrique('Exemple');
    
    add_filter('mcb_sanitize_options', array($exemple, 'filter_mcb_sanitize_options'), 10, 3);
    
    if(is_admin() && $settings && current_user_can($settings::SETTINGS_CAPABILITY)){
    	add_action('mcb_settings_display_tab_autrebrique', array($exemple, 'mcb_settings_display_tab_autrebrique'));
    }

.. note::
    *L'onglet d'administration "autrebrique" n'existe pas par défaut, il s'agit simplement d'un exemple. Pour l'exemple, l'onglet "autrebrique" est rattaché à la Brique "AutreBrique".*

Voici les fonctions liées au filtre et à l'action (**à insérer dans la Brique Exemple**) :

.. code-block:: php

    <?php
    	/**
    	 * Filtrage des options
    	 *
    	 * @filter mcb_sanitize_options
    	 *
    	 * @param array $cleanOptions
    	 *      Options validées
    	 * @param string $brique
    	 *      Intitulé de la brique concernée
    	 * @param array $options
    	 *      Options à valider
    	 *
    	 * @return array
    	 *      Options validées
    	 */
    	public static function filter_mcb_sanitize_options($cleanOptions, $brique, $options){
    		if($brique == 'AutreBrique'){
    			if(!is_array($cleanOptions))
    				$cleanOptions = array();
    			$cleanOptions['exemple'] = (isset($options['exemple']) && $options['exemple'] == 1)? 1 : 0;
    		}
    		return $cleanOptions;
    	}
    	/**
    	 * Affichage des options de l'onglet admin Autre Brique
    	 *
    	 * @action mcb_settings_display_tab_autrebrique
    	 */
    	public static function mcb_settings_display_tab_autrebrique(){
    		static::loadTemplate('Settings/autrebrique.php', array(), false);
    	}

.. note::
    *La Brique "AutreBrique" n'existe pas par défaut, il s'agit simplement d'un exemple.*

Le fichier de template :

``/wp-content/plugins/mc_briques_exemple/templates/Exemple/Settings/autrebrique.php``

.. code-block:: php

    <?php
    /**
     * Onglet admin Commandes : Reversements
     */
    if(!defined('ABSPATH'))
    	exit;
    
    $autreBrique = MCBriques::loadBrique('AutreBrique');
    
    ?><table class="form-table">
    	<tbody>
    		<tr>
    			<th>Exemple</th>
    			<td><input type="checkbox" name="<?php echo esc_attr($autreBrique::OPTIONS); ?>[exemple]" value="1"<?php if($autreBrique::getOption('exemple')){ ?> checked="checked"<?php } ?> /></td>
    		</tr>
    	</tbody>
    </table>

.. note::
    *La Brique "AutreBrique" n'existe pas par défaut, il s'agit simplement d'un exemple.*

Récupération des options d'une Brique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php
    $exemple = MCBriques::loadBrique('Exemple');
    
    // Récupère un tableau associatif des options de la Brique Exemple
    $options = $exemple::getOptions();
    
    // Récupère l'option "exemple" de la Brique Exemple
    $optionExemple = $exemple::getOption('exemple');

.. _override-brique:

Override/Remplacement d'une Brique existante
--------------------------------------------

Il est possible de surcharger une Brique partiellement ou totalement. Une surcharge partielle correspond à un héritage et permet d'altérer la Brique d'origine sans recoder l'ensemble de la Brique.

Surcharge partielle
~~~~~~~~~~~~~~~~~~~

Voici le code complet d'un plugin permettant de surcharger partiellement la Brique Exemple :

``/wp-content/plugins/mc_briques_exemple_override/mc_briques_exemple_override.php``

.. code-block:: php

    <?php
    /**
     * Plugin Name: Médias Cité : BRIQUES Exemple Override
     * Description: Ajoute la Brique Exemple Override
     * Version: 1.0
     * Author: Cogitime
     * License: GPL2
     * Licence URI: https://www.gnu.org/licenses/gpl-2.0.html
     * Text Domain: mcbriques
     * Domain Path: /languages
     *
     * @package Briques
     * @category ExempleOverride
     * @author Cogitime
     */
    
    if(!defined('ABSPATH'))
    	exit;
    
    define('MCBRIQUES_EXEMPLE_OVERRIDE_PLUGIN_FILE', __FILE__);
    
    if(!class_exists('MCBriquesExempleOverride')){
    	require_once(dirname(MCBRIQUES_EXEMPLE_OVERRIDE_PLUGIN_FILE).'/classes/MCBriquesExempleOverride.php');
    	MCBriquesExempleOverride::init();
    }

``/wp-content/plugins/mc_briques_exemple_override/classes/MCBriquesExempleOverride.php``

.. code-block:: php

    <?php
    /**
     * Classe principale du plugin ExempleOverride
     */
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Classe principale du plugin ExempleOverride
     *
     * @class 		MCBriquesExempleOverride
     * @category	Class
     * @package 	Briques\Classes
     */
    class MCBriquesExempleOverride
    {
    	protected static $overrideConfig = array();
    	/**
    	 * Initialisation du plugin
    	 */
    	public static function init(){
    		// Action plugins_loaded en priorité 11 afin d'attendre que tous les plugins Briques soient chargés
    		add_action('plugins_loaded', array(__CLASS__, 'action_pluginsLoaded'), 11);
    	}
    	
    	/**
    	 * Action WP plugins_loaded
    	 */
    	public static function action_pluginsLoaded(){
    		if(class_exists('MCBriques')){
    			$briquesPath = dirname(__FILE__).'/briques/';
    			
    			// 1. Chargement de la Brique à surcharger
    			if($exemple = MCBriques::loadBrique('Exemple')){
    				// 2. Enregistrement de la configuration actuelle de la Brique Exemple
    				$config = self::setOverrideConfig('Exemple');
    				// 3. Création d'un alias pour la classe de la Brique Exemple
    				class_alias($exemple, 'MCBriqueExempleOverrideFixExtends');
    				// 4. Déchargement de la brique Exemple
    				MCBriques::unloadBrique('Exemple');
    				// 5. Enregistrement de la nouvelle Brique Exemple
    				MCBriques::registerBrique('Exemple', $config['plugin'], $briquesPath.'ExempleOverride.php', 'MCBriqueExempleOverride');
    			}
    		}
    	}
    	
    	protected static function setOverrideConfig($brique, $config=false){
    		return self::$overrideConfig[$brique] = (empty($config))? MCBriques::getBriqueConfig($brique) : $config;
    	}
    	public static function getOverrideConfigs(){
    		return self::$overrideConfig;
    	}
    	public static function getOverrideConfig($brique){
    		return (isset(self::$overrideConfig[$brique]))? self::$overrideConfig[$brique] : false;
    	}
    }

.. note::
    1. La Brique à surcharger doit être chargée

    2. Il est nécessaire d'enregistrer la configuration de la Brique au moment de la surcharge puisqu'il est possible de surcharger plusieurs fois la même Brique
    
    3. Il est possible que la Brique d'origine soit déjà surchargée auquel cas le nom de classe de la Brique n'est plus celui d'origine, il faut donc créer un alias afin de pouvoir hériter d'un nom de classe connu
    
    4. Afin de permettre de recharger la Brique il faut dans un premier temps la décharger
    
    5. Enregistrement de la nouvelle configuration de la Brique

``/wp-content/plugins/mc_briques_exemple_override/classes/briques/ExempleOverride.php``

.. code-block:: php

    <?php
    /**
     * Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Brique Exemple
     *
     * @class 		MCBriqueExempleOverride
     * @category	Class
     * @package 	Briques\Classes\Brique
     */
    class MCBriqueExempleOverride extends MCBriqueExempleOverrideFixExtends
    {
    	
    }

``/wp-content/plugins/mc_briques_exemple_override/classes/briques/ExempleOverride_init.php``

.. code-block:: php

    <?php
    /**
     * Initialisation de la Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    $exemple = MCBriques::loadBrique('Exemple');
    
    $overrideConfig = MCBriquesExempleOverride::getOverrideConfig('Exemple');
    if(is_array($overrideConfig) && isset($overrideConfig['file'])){
    	$initFile = $exemple::addFilenameSuffix($overrideConfig['file'], '_init');
    	if(file_exists($initFile))
    		require_once($initFile);
    }

.. note::
    Chargement du fichier d'initialisation de la brique d'origine. Ce chargement permet de garder les fichiers d'initialisations des surcharges.
    
    Sans ce chargement, seul votre nouveau fichier d'initialisation serait chargé (si votre fichier n'existe pas alors aucun fichier d'initialisation ne sera chargé).

Surcharge totale (remplacement d'une Brique)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Une surcharge totale est plus simple à mettre en place qu'une surcharge partielle car il n'est pas nécessaire de gérer l'héritage.

Voici le plugin mc_briques_exemple_override en surcharge totale :

``/wp-content/plugins/mc_briques_exemple_override/mc_briques_exemple_override.php``

.. code-block:: php

    <?php
    /**
     * Plugin Name: Médias Cité : BRIQUES Exemple Override
     * Description: Ajoute la Brique Exemple Override
     * Version: 1.0
     * Author: Cogitime
     * License: GPL2
     * Licence URI: https://www.gnu.org/licenses/gpl-2.0.html
     * Text Domain: mcbriques
     * Domain Path: /languages
     *
     * @package Briques
     * @category ExempleOverride
     * @author Cogitime
     */
    
    if(!defined('ABSPATH'))
    	exit;
    
    define('MCBRIQUES_EXEMPLE_OVERRIDE_PLUGIN_FILE', __FILE__);
    
    if(!class_exists('MCBriquesExempleOverride')){
    	require_once(dirname(MCBRIQUES_EXEMPLE_OVERRIDE_PLUGIN_FILE).'/classes/MCBriquesExempleOverride.php');
    	MCBriquesExempleOverride::init();
    }

``/wp-content/plugins/mc_briques_exemple_override/classes/MCBriquesExempleOverride.php``

.. code-block:: php

    <?php
    /**
     * Classe principale du plugin ExempleOverride
     */
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Classe principale du plugin ExempleOverride
     *
     * @class 		MCBriquesExempleOverride
     * @category	Class
     * @package 	Briques\Classes
     */
    class MCBriquesExempleOverride
    {
    	/**
    	 * Initialisation du plugin
    	 */
    	public static function init(){
    		// Action plugins_loaded en priorité 11 afin d'attendre que tous les plugins Briques soient chargés
    		add_action('plugins_loaded', array(__CLASS__, 'action_pluginsLoaded'), 11);
    	}
    	
    	/**
    	 * Action WP plugins_loaded
    	 */
    	public static function action_pluginsLoaded(){
    		if(class_exists('MCBriques')){
    			$briquesPath = dirname(__FILE__).'/briques/';
    			
    			MCBriques::registerBrique('Exemple', 'mc_briques_exemple_override', $briquesPath.'ExempleOverride.php', 'MCBriqueExempleOverride');
    		}
    	}
    }

``/wp-content/plugins/mc_briques_exemple_override/classes/briques/ExempleOverride.php``

.. code-block:: php

    <?php
    /**
     * Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Brique Exemple
     *
     * @class 		MCBriqueExempleOverride
     * @category	Class
     * @package 	Briques\Classes\Brique
     */
    class MCBriqueExempleOverride extends MCBrique
    {
    	const BRIQUE = 'Exemple';
    }

``/wp-content/plugins/mc_briques_exemple_override/classes/briques/ExempleOverride_init.php``

.. code-block:: php

    <?php
    /**
     * Initialisation de la Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;