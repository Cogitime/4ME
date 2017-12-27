Utilisation des mails
=====================

La gestion des mails est ajoutée par la brique **Mail** présente dans le plugin ``mc_briques_mail``.

Ajouter un mail
---------------

Voici un exemple de code permettant d’ajouter un mail dans la brique Exemple :

``/wp-content/plugins/mc_briques_exemple/classes/briques/Exemple_init.php``

.. code-block:: php

    <?php
    /**
     * Initialisation de la Brique Exemple
     */
    if(!defined('ABSPATH'))
        exit;
    
    if(MCBriques::loadBrique('Mail')){
    	$exemple = MCBriques::loadBrique('Exemple');
    	$exemple::registerMail('exemple', 'Mail/exemple.php');
    }

.. warning::
    Le template du mail est configuré sur ``Mail/exemple.php`` mais le template réellement utilisé sera ``Mail/exemple_html.php`` si l'envoi HTML est activé, sinon ``Mail/exemple_text.php``

``/wp-content/plugins/mc_briques_exemple/templates/Exemple/Mail/exemple_html.php``

.. code-block:: php

    <?php
    
    if(!defined('ABSPATH'))
    	exit;
    
    ?>
    <p>Bonjour,</p>
    <table class="table" style="width:100%">
    	<tr>
    		<td class="box" style="width:100%;border:1px solid #D6D4D4;background-color:#f8f8f8;padding:7px 10px">
    			<font size="2" face="Open-sans, sans-serif" color="#555454">
    				<p style="border-bottom:1px solid #D6D4D4;margin:3px 0 7px;text-transform:uppercase;font-weight:500;font-size:18px;padding-bottom:10px">Exemple de mail</p>
    				<span style="color:#777">Ceci est un mail d'exemple</span>
    			</font>
    		</td>
    	</tr>
    </table>

.. note::
    Le code HTML utilisé pour rédiger le mail doit être compatible avec un maximum de clients mail.

``/wp-content/plugins/mc_briques_exemple/templates/Exemple/Mail/exemple_text.php``

.. code-block:: php

    <?php
    
    if(!defined('ABSPATH'))
    	exit;
    
    ?>
    Bonjour,
    
    Exemple de mail
    ---------------
    Ceci est un mail d'exemple


Envoyer un mail
---------------

Voici un code d'exemple permettant de procéder à l'envoi d'un mail depuis une fonction dans la brique Exemple :

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
     * @class           MCBriqueExemple
     * @category        Class
     * @package         Briques\Classes\Brique
     */
    class MCBriqueExemple extends MCBrique
    {
        const BRIQUE = 'Exemple';
    	
    	public static function maFonctionDEnvoiDuMail($toEmail){
    		if(static::loadBrique('Mail')){
    			if(static::sendMail($toEmail, 'Sujet du mail', 'exemple')){
    				static::addNotice('Mail envoyé avec succès', 'success');
    			}else{
    				static::addNotice('Erreur lors de l\'envoi du mail', 'warning');
    			}
    		}
    	}
    }

.. note::
    Comme pour l'utilisation des templates, il est possible de passer des paramètres au template sous forme d'un tableau associatif en 4ème paramètre de la fonction **sendMail**