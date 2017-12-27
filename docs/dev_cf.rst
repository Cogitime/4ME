Champs personnalisés
====================

Le système de champs personnalisés est ajouté par la brique **CustomField** présente dans le plugin ``mc_briques_cf``.

Les champs personnalisés sont configurables sur les annonces et sur les utilisateurs. Par défaut, seuls deux types de champ sont disponibles (texte et sélection).


Créer un nouveau type de champ personnalisé
-------------------------------------------

Voir le plugin ``mc_briques_geoloc`` pour un exemple concret de l'ajout d'un champ de type Géolocalisation (basé sur l'API Google).

Voici un exemple de code permettant d'ajouter un type de champ personnalisé dans la brique **Exemple** :

``/wp-content/plugins/mc_briques_exemple/classes/briques/Exemple_init.php``

.. code-block:: php

    <?php
    /**
     * Initialisation de la Brique Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    $customField = MCBriques::loadBrique('CustomField');
    $exemple = MCBriques::loadBrique('Exemple');
    if($customField && $exemple){
    	$fieldsPath = dirname(__FILE__).'/fields/';
    	require_once($fieldsPath.'Exemple.php');
    }

``/wp-content/plugins/mc_briques_exemple/classes/briques/fields/Exemple.php``

.. code-block:: php

    <?php
    /**
     * Brique Exemple : Champ Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    /**
     * Brique Exemple : Champ Exemple
     *
     * @class 		MCBriqueCustomField_Exemple
     * @category	Class
     * @package 	Briques\Classes\Brique\CustomField
     */
    if(class_exists('MCBriqueCustomField_Abstract')){
    	class MCBriqueCustomField_Exemple extends MCBriqueCustomField_Abstract
    	{
    		// Nom du type de champ personnalisé
    		protected static $field = 'Exemple';
    		// Configuration du type de champ ( voir la fonction MCBriqueCustomField_Abstract::getConfig() )
    		protected static $config = array(
    			'label' => 'Champ exemple',
    			'brique' => 'Exemple'
    		);
    		
    		/**
    		 * Filtrage de la valeur du champ
    		 */
    		public static function sanitize_value($value, $cleanValue=false){
    			if($cleanValue === false)
    				$cleanValue = $value;
    			return parent::sanitize_value($value, (is_string($cleanValue))? sanitize_text_field($cleanValue) : '');
    		}
    	}
    	MCBriqueCustomField_Exemple::init();
    }

.. note::
    Une configuration par défaut est définit dans la fonction **MCBriqueCustomField_Abstract::getConfig()**.
    
    La fonction **sanitize_value** permet de filtrer la valeur du champ afin de retourner une valeur correspondant au type attendu. Dans cet exemple nous limitons le champ à une chaine de caractères.

``/wp-content/plugins/mc_briques_exemple/templates/Exemple/fields/Exemple/field.php``

.. code-block:: php

    <?php
    /**
     * Affichage du champ : Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    if(!empty($field) && !empty($config) && !empty($fieldSettings) && isset($metas) && isset($terms)){
    	$customField = MCBriques::loadBrique('CustomField');
    	$fieldSettings = $customField::sanitizeFieldSettings($fieldSettings);
    	$key = 'mcb_cf_'.$fieldSettings['name'];
    	$fieldValue = (!empty($metas[$key]))? $metas[$key][0] : '';
    	if($customField::getField($field) && $fieldValue != ''){
    ?>
    	<div class="field">
    		<span class="field_label"><?php echo esc_html($fieldSettings['label']); ?></span>
    		<span class="field_value"><?php echo esc_html($fieldValue); ?></span>
    	</div>
    <?php
    	}
    }

.. note::
    Ce template correspond à l'affichage du champ sur le site (sur l'annonce ou sur le profil de l'utilisateur)


``/wp-content/plugins/mc_briques_exemple/templates/Exemple/fields/Exemple/form.php``

.. code-block:: php

    <?php
    /**
     * Affichage du champ dans un formulaire : Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    if(!empty($field) && !empty($config) && !empty($fieldSettings) && is_array($fieldSettings) && isset($metas) && isset($terms)){
    	$customField = MCBriques::loadBrique('CustomField');
    	if($customField::getField($field)){
    		$fieldSettings = $customField::sanitizeFieldSettings($fieldSettings);
    		$key = 'mcb_cf_'.$fieldSettings['name'];
    		
    		$fieldValue = (isset($_POST[$key]))? $_POST[$key] : ((!empty($metas[$key]))? $metas[$key][0] : '');
    ?>
    	<label for="mcb_cf_<?php echo esc_attr($fieldSettings['name']); ?>"><?php echo esc_html($fieldSettings['label']); ?></label>
    	<input type="text" name="mcb_cf_<?php echo esc_attr($fieldSettings['name']); ?>" id="mcb_cf_<?php echo esc_attr($fieldSettings['name']); ?>" class="form-control" value="<?php echo esc_attr($fieldValue); ?>"<?php if(!empty($fieldSettings['required'])){ ?> required<?php } ?> />
    <?php
    	}
    }

.. note::
    Ce template correspond à l'affichage du champ dans un formulaire

``/wp-content/plugins/mc_briques_exemple/templates/Exemple/fields/Exemple/search.php``

.. code-block:: php

    <?php
    /**
     * Affichage du champ dans une recherche : Exemple
     */
    if(!defined('ABSPATH'))
    	exit;
    
    if(!empty($field) && !empty($config) && !empty($fieldSettings) && is_array($fieldSettings)){
    	$customField = MCBriques::loadBrique('CustomField');
    	if($customField::getField($field)){
    		$fieldSettings = $customField::sanitizeFieldSettings($fieldSettings);
    		$searchValue = (isset($_GET['cf_'.$fieldSettings['name']]))? $_GET['cf_'.$fieldSettings['name']] : '';
    ?>
    	<?php if(!empty($fieldSettings['label'])){ ?><label for="cf_<?php echo esc_attr($fieldSettings['name']); ?>"><?php echo esc_attr($fieldSettings['label']); ?></label><?php } ?>
    	<input type="text" name="cf_<?php echo esc_attr($fieldSettings['name']); ?>" id="cf_<?php echo esc_attr($fieldSettings['name']); ?>" class="form-control"<?php if(!empty($fieldSettings['placeholder'])){ ?> placeholder="<?php echo esc_attr($fieldSettings['placeholder']); ?>"<?php } ?> value="<?php echo esc_attr($searchValue); ?>" />
    <?php
    	}
    }

.. note::
    Ce template correspond à l'affichage du champ dans le formulaire de recherche

Cet exemple intègre simplement un type de champ texte (nommé Exemple) entièrement similaire au type de champ texte présent par défaut. Il est cependant possible de créer n'importe quel type de champ via ce système.

Pour plus d'informations sur les différentes fonctions disponibles, consulter la classe abstraite **MCBriqueCustomField_Abstract**.