Module de paiement (Brique Commande)
====================================

La brique "Commande" intègre un système de modules de paiement paramétrables afin d'ajouter divers moyens de paiement pour finaliser une commande.

Seul le mode de paiement par chèque est présent par défaut avec le plugin de la brique "Commande".

Créer un module de paiement
---------------------------

Voir le plugin ``mc_briques_virement`` afin de voir un exemple concret d'un module de paiement minimaliste (le paiement par virement fonctionne exactement comme le paiement par chèque, il permet simplement de finaliser le commande en affichant les instructions de paiement).

Voir le plugin ``mc_briques_paypal`` pour voir l'intégration d'une plateforme de paiement externe.

Voici un exemple de code pour ajouter un module de paiement (lié à une plateforme de paiement) dans la brique "Exemple" :

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
    		add_action('init', array(__CLASS__, 'action_init'), 0);
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
    	
    	/**
    	 * Action WP init
    	 */
    	public static function action_init(){
    		if(class_exists('MCBriques') && ($paiement = MCBriques::loadBrique('Paiement'))){
    			$paiementsPath = dirname(__FILE__).'/paiements/';
    			
    			$paiement::registerPaiement('exemple', 'mc_briques_exemple', $paiementsPath.'Exemple.php', 'MCBPaiementExemple');
    		}
    	}
    }

.. note::
    L'enregistrement des paiements est effectué lors de l'action WordPress **init** en priorité 0 afin d'être chargé avant l'initialisation générale des autres plugins.

``/wp-content/plugins/mc_briques_exemple/classes/paiements/Exemple_init.php``

.. code-block:: php

    <?php
    
    if(!defined('ABSPATH'))
    	exit;
    
    if($paiement = MCBriques::loadBrique('Paiement') && $paiementClass = $paiement::loadPaiement('exemple')){
    	add_action('wp', array($paiementClass, 'action_wp'));
    	add_action('mcb_api_mcb_gateway_exemple', array($paiementClass, 'mcb_api_mcb_gateway_exemple'));
    }

``/wp-content/plugins/mc_briques_exemple/classes/paiements/Exemple.php``

.. code-block:: php

    <?php
    
    if(!defined('ABSPATH'))
    	exit;
    
    /**
     * Brique Paiement Exemple
     *
     * @class 		MCBPaiementExemple
     * @category	Class
     * @package 	Briques\Classes\Brique\Paiement
     */
    if(class_exists('MCBPaiement')){
    	class MCBPaiementExemple extends MCBPaiement
    	{
    		const BRIQUE = 'Exemple';
    		const PAIEMENT = 'exemple';
    
    		/**
    		 * Vérifie si le mode de paiement est actif et opérationnel
    		 */
    		public static function checkAvailable($panier=false, $commande=false){
    			if(!parent::checkAvailable($panier, $commande))
    				return false;
    			
    			return (static::getOption('email') != '')? true : false;
    		}
    		
    		/**
    		 * Déclenchement du paiement
    		 */
    		public static function processPaiement($commandeID){
    			$commande = static::loadBrique('Commande');
    			if(!$commande || empty($commandeID) || get_post_type($commandeID) != $commande::POST_TYPE)
    				return array();
    			
    			$commandePost = get_post($commandeID);
    			
    			$panierUrl = get_permalink($commande::getPageTemplatePageId('panier.php'));
    			
    			// Redirection vers la plateforme de paiement avec tous les paramètres nécessaires
    			wp_redirect('https://www.exemple.fr/cgi-bin/webscr?'.http_build_query(array(
    				'business' => static::getOption('email'),
    				'order_id' => $commandeID,
    				
    				// Adresse de retour après avoir finalisé le paiement
    				'return' => esc_url_raw(add_query_arg(array(
    					'exemple' => 'return',
    					'id' => $commandePost->ID,
    					'nonce' => wp_create_nonce('exemple_return_'.$commandePost->ID)
    				), $panierUrl)),
    				
    				// Adresse de retour en cas d'annulation
    				'cancel' => esc_url_raw(add_query_arg(array(
    					'exemple' => 'cancel',
    					'id' => $commandePost->ID,
    					'nonce' => wp_create_nonce('exemple_cancel_'.$commandePost->ID)
    				), $panierUrl)),
    				
    				// Adresse de notification pour que la plateforme de paiement communique avec le module de paiement (voir l'action enregistrée dans le fichier Exemple_init.php
    				'notify_url' => static::addQueryArg('mcb-api', 'MCB_Gateway_Exemple', home_url())
    			));
    			die();
    		}
    		
    		/**
    		 * Action WP wp
    		 * Traitement du retour de l'interface de paiement
    		 */
    		public static function action_wp(){
    			$panierPageId = static::getPageTemplatePageId('panier.php');
    			if(!empty($panierPageId) && $panierPageId == get_the_ID()){
    				if(isset($_GET['exemple']) && is_string($_GET['exemple']) && in_array($_GET['exemple'], array('return', 'cancel'), true) && isset($_GET['id']) && is_numeric($_GET['id'])){
    					$commande = static::loadBrique('Commande');
    					if(get_post_type($_GET['id']) == $commande::POST_TYPE && !empty($_GET['nonce']) && is_string($_GET['nonce']) && wp_verify_nonce($_GET['nonce'], 'exemple_'.$_GET['exemple'].'_'.$_GET['id']) && in_array(get_post_status($_GET['id']), $commande::getCartAllowStatus())){
    						/**
    						 * /!\ ATTENTION : Le paiement de la commande ne doit pas être validée ici !!!! Il s'agit simplement du traitement du retour du client via son navigateur internet. La validation du paiement doit être effectuée via le retour de la banque (mcb_api_mcb_gateway_exemple dans cet exemple)
    						 */
    						switch($_GET['exemple']){
    							case 'return':
    								// On vide le panier et on affiche la confirmation de commande
    								$commande::resetPanier();
    								$commande::redirectConfirmationCommande($_GET['id']);
    							break;
    							case 'cancel':
    								// Afin de débloquer la disponibilité du produit on annule simplement la commande
    								$commande::updateCommandeStatus($_GET['id'], 'mcbc-canceled');
    							break;
    						}
    					}
    				}
    			}
    		}
    		
    		/**
    		 * Traitement du retour de la banque
    		 */
    		public static function mcb_api_mcb_gateway_exemple(){
    			// Vérification du retour de la banque et mise à jour de la commande concernée
    			// Voir la doc et/ou le script d'intégration du kit fourni par la banque
    		}
    		
    		/**
    		 * Filtrage des options du mode de paiement
    		 */
    		public static function sanitize_options($options, $cleanOptions=array()){
    			if(!is_array($options))
    				$options = array();
    			if(!is_array($cleanOptions))
    				$cleanOptions = array();
    			
    			if(!isset($cleanOptions['email']))
    				$cleanOptions['email'] = (isset($options['email']) && is_email($options['email']))? $options['email'] : '';
    			
    			return parent::sanitize_options($options, $cleanOptions);
    		}
    	}
    }

.. note::
    Ce module de paiement n'est pas complet puisqu'il s'agit d'un exemple et qu'il n'intègre donc pas de kit bancaire.
    
    **Il est cependant important de noter que la validation du paiement ne doit pas être effectuée sur une action de déclenchée par l'utilisateur mais sur une action déclenchée par la banque !**
    Pour cela il est nécessaire d'intégrer correctement le kit fourni par la banque.

``/wp-content/plugins/mc_briques_exemple/templates/Exemple/exemple/settings.php``

.. code-block:: php

    <?php
    
    if(!defined('ABSPATH'))
    	exit;
    
    $paiement = MCBriques::loadBrique('Paiement');
    $paiementClass = $paiement::loadPaiement($paiementCode);
    
    if($paiementClass){
    	$optionsName = 'mcb_paiement_'.strtolower($paiementClass::PAIEMENT).'_options';
    	
    	?>
    	<table class="form-table">
    		<tbody>
    			<tr>
    				<th>Adresse email</th>
    				<td><input type="email" name="<?php echo esc_attr($optionsName); ?>[email]" value="<?php echo esc_attr($paiementClass::getOption('email')); ?>" /></td>
    			</tr>
    		</tbody>
    	</table>
    	<?php
    }