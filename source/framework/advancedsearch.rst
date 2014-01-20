====================
La recherche avancée
====================

Les différents types de recherche
=================================

Recherche simple
----------------

Cette recherche est celle disponible par défaut sur les tableaux d'openMairie.

Elle permet de:

- rechercher une valeur dans une colonne parmi toutes celles affichées;
- rechercher une valeur dans toutes les colonnes affichées;
- rechercher des valeurs approximatives.

.. note::
   Il est possible de modifier la liste des colonnes dans laquelle est effectuée
   la recherche. Cette liste ne correspond pas forcément aux colonnes
   affichées. Elle correspond seulement par défaut, c'est à dire lorsqu'aucune
   surcharge ne modifie les fichiers générés dans ``gen/sql/``.

Recherche avancée
-----------------

Cette recherche est une fonctionnalité qui peut être activée et configurée
manuellement pour un ou plusieurs tableaux donnés.

Elle permet de:

- afficher un formulaire de recherche mono-critère permettant d'effectuer des
  recherches strictes ou approximatives;
- afficher un formulaire de recherche multi-critères permettant d'effectuer
  des recherches strictes ou approximatives;
- rechercher des valeurs dans des tables et des colonnes qui ne sont pas
  affichées.

Recherche avancée mono-critère
..............................

Le formulaire de recherche mono-critère est un formulaire ne s'affichant que si
la recherche avancée est activée. Il permet aux utilisateurs de basculer sur un
formulaire similaire à celui de recherche simple lorsque la recherche avancée
est activée.

Ce formulaire se comporte de la même manière que celui de recherche simple, avec
quelques différences:

- il permet de rechercher des valeurs strictes ou approximatives (par défaut
  approximatives);
- il recherche dans toutes les colonnes proposées par la recherche simple;
- il conserve les valeurs recherchées après la réalisation d'une action (ajout,
  modification, etc...);
- il dispose d'un bouton ``Vider le formulaire`` permettant de vider les champs;
- il dispose d'un bouton ``+`` permettant de basculer sur le formulaire
  multi-critères.

Recherche avancée multi-critères
................................

Le formulaire de recherche multi-critères est un formulaire ne s'affichant que
si la recherche avancée est activée. Il permet aux utilisateurs de bénéficier de
plusieurs champs, et ainsi effectuer des recherches plus précise qu'avec le
formulaire de recherche simple.

Description du formulaire:

- il peut afficher plusieurs champs, de type texte, nombre, date ou liste à
  choix;
- il permet, pour chaque tableau, de configurer la liste des champs affichés;
- il permet, pour chaque champ, de rechercher des valeurs strictes ou
  approximatives (par défaut approximatives);
- il permet, pour chaque champ, de rechercher des valeurs dans des tables et
  des colonnes qui ne sont pas affichées;
- il conserve les valeurs recherchées après la réalisation d'une action (ajout,
  modification, etc...);
- il dispose d'un bouton ``Vider le formulaire`` permettant de vider les champs;
- il dispose d'un bouton ``+`` permettant de basculer sur le formulaire
  mono-critère.

Configuration de la recherche avancée
=====================================

Activation
----------

Exemple avec le modèle ``om_utilisateur``.

Pour activer la recherche avancée, rendez-vous dans le fichier
``sql/sgbd/om_utilisateur.inc.php`` et ajoutez la configuration suivante au
tableau d'options:

.. code-block:: php

   <?php

    $options[] =  array('type' => 'search',
                        'display' => true,
                        'advanced' => $champs,
                        'default_form' => 'advanced',
                        'absolute_object' => 'om_utilisateur');

   ?>

.. note::
   A partir de la version 4.3.0 d'openMairie, le tableau ``$options`` est
   disponible dans les fichiers ``sql/`` de l'application. Il n'est plus
   nécessaire de le déclarer manuellement.

La clé ``type`` est obligatoire. Elle permet de définir le type de l'option.
Pour une recherche il faut saisir ``search``.

La clé ``display`` est obligatoire. Elle permet d'afficher ou non la recherche,
tout en conservant sa configuration.

- ``true`` permet d'afficher la recherche;
- ``false`` permet de masquer la recherche.

La clé ``advanced`` est obligatoire (pour la recherche avancée). Elle permet de
préciser que le formulaire de recherche est un formulaire de recherche avancée
et non simple. Cette clé doit contenir le tableau des champs configurés pour la
recherche (voir plus bas pour la configuration des champs).

La clé ``default_form`` est optionnelle. Elle permet de choisir quel formulaire
de recherche est ouvert par défaut. La valeur ``advanced'`` permet d'afficher le
formulaire multi-critères. Les autres valeurs, ou si ``default_form`` n'est pas
configuré, affichent le formulaire mono-critère.

La clé ``absolute_object`` est obligatoire. Elle permet de spécifier à
openMairie le nom du modèle l'objet recherché. Ce nom est celui du fichier dans
``obj/``, ici ``om_utilisateur.class.php`` (sans son extension).

Autres paramètres
-----------------

**Wildcard**

Le wildcard permet de rendre la recherche stricte ou approximative.

Cette option peut se configurer pour un ou plusieurs modèles particuliers dans
les fichiers correspondants du répertoire ``sql/`` de l'application. Elle peut
également être configurée de manière globale pour l'ensemble dans modèle
à partir du fichier ``dyn/tab.inc.php``.

Par défaut, il est paramétré de la manière suivante:

.. code-block:: php

   <?php

   $options[] = array('type' => 'wildcard', 'left' => '%', 'right' => '%');

   ?>

- ``left`` détermine, dans la requête SQL de recherche, le caractère ajouté au
  début (à gauche) de la valeur recherchée;
- ``right`` détermine, dans la requête SQL de recherche, le caractère ajouté en
  fin (à droite) de la valeur recherchée.

Avec cette configuration lorsque le mot « admin » est recherché dans une
colonne, toutes les valeurs contenant « admin » sont retournées.

En modifiant la configuration de cette manière:

.. code-block:: php

   <?php

   $options[] = array('type' => 'wildcard', 'left' => '', 'right' => '%');

   ?>

Seules les valeurs **commençant** par « admin » seront retournées.


Enfin avec:

.. code-block:: php

   <?php

   $options[] = array('type' => 'wildcard', 'left' => '', 'right' => '');

   ?>

Seules les valeurs égales **exactement** à « admin » seront retournées.

Configuration des critères de recherche
=======================================

La recherche avancée ne fonctionnera pas tant que la liste des champs du
formulaire multi-critères n'aura pas été créée. Ces champs sont appelés ici des
critères de recherche.

Configuration simple
--------------------

Un critère de recherche est représenté par un tableau PHP contenant sa
configuration.

.. code-block:: php

   <?php

   $champs['identifiant_utilisateur'] =
       array('colonne' => 'om_utilisateur',
             'table' => 'om_utilisateur',
             'type' => 'text',
             'libelle' => _('Identifiant'),
             'taille' => 10,
             'max' => 8));

   ?>


La clé ``identifiant_utilisateur`` est le nom du champ HTML qui sera affiché
sur le formulaire.

La clé ``colonne`` est obligatoire. Elle contient le nom de la colonne de la
base de données qui sera interrogée si la variable ``$_POST`` contient la clé
``identifiant_utilisateur``.

La clé ``table``  est obligatoire. Elle contient le nom de la table de la base
de données qui sera interrogée si la variable ``$_POST`` contient la clé
``identifiant_utilisateur``.

La clé ``'type`` est obligatoire. Elle contient le type du champ HTML à
afficher. Cela peut être ``date``, ``text``, ``select``, ou tout autre méthode
de la classe ``formulaire``. Pour les champs de type ``select``, le nom du champ
HTML doit être le même que le nom de la colonne.

La clé ``libelle`` est obligatoire. Elle contient le libellé qui sera affiché à
côté du champ dans le formulaire de recherche.

La clé ``taille`` est optionnelle. Elle contient la taille du champ HTML
(attribut HTML ``size``).

La clé ``max`` est optionnelle. Elle contient la longueur maximale de la valeur
du champ HTML (attribut HTML ``maxlength``).

Une fois tous les critères de recherche configurés, il faudra simplement
vérifier que le tableau des critères est bien utilisé par l'option de type
``search``.

Exemple de formulaire pour le tableau du modèle ``om_utilisateur``:

.. code-block:: php

   <?php

   $champs = array();

   $champs['login'] = array(
       'table' => 'om_utilisateur',
       'colonne' => 'login',
       'type' => 'text',
       'libelle' => _('Login'));
   
   $champs['email'] = array(
       'table' => 'om_utilisateur',
       'colonne' => 'email',
       'type' => 'text',
       'libelle' => _('E-mail'));
   
   $champs['om_profil'] = array(
       'table' => 'om_utilisateur',
       'colonne' => 'om_profil',
       'type' => 'select',
       'libelle' => _('Profil'));

    $options[] =  array('type' => 'search',
                        'display' => true,
                        'advanced' => $champs,
                        'default_form' => 'advanced',
                        'absolute_object' => 'om_utilisateur');

   ?>

Configuration avancée
---------------------

Créer un intervalle de date
...........................

Exemple: recherche des utilisateurs crées entre telle et telle date.

.. code-block:: php

   <?php

   $champs['date_de_creation'] =
       array('colonne' => 'creation_date',
             'table' => 'user',
             'libelle' => _('Date de creation'),
             'type' => 'date',
             'where' => 'intervaldate');

   ?>

Cette configuration permet de créer deux champs HTML ``datepicker``:

- ``date_de_creation_min`` : permettra de saisir une date minimale
- ``date_de_creation_max`` : permettra de saisir une date maximale

Ces champs permettent de rechercher les utilisateurs dont la date de créations
est incluse dans l'intervalle saisi, bornes comprises. Il est possible de ne
saisir qu'une seule date afin de rechercher les utilisateurs ayant été créés
avant ou après une date particulière.

Créer un champ de recherche avec menu déroulant personnalisé
............................................................

Exemple: recherche des utilisateurs administrateurs.

Dans cet exemple, l'information se trouve directement dans la table interrogée.

.. code-block:: php

   <?php

   // soit 'user' une table contenant une colonne 'is_admin'

   $args = array();
   $args[0] = array('', 'true', 'false');
   $args[1] = array(_('Tous'), _('Oui'), _('Non'));

   $champs['administrator'] =
       array('colonne' => 'is_admin',
             'table' => 'user',
             'libelle' => _('Administrateur'),
             'type' => 'select',
             'subtype' => 'manualselect',
             'args' => $args);

   ?>

Cette configuration permet de créer un champ HTML de type ``select`` avec trois
choix:

- Tous (valeur '');
- Oui (valeur ``true``);
- Non (valeur ``false``).

Le tableau ``$args[0]`` contient les valeurs associées aux choix. Elles seront
recherchées telles quelles dans la base de données.

En sélectionnant « Oui », la requête SQL de recherche sera construite comme
suit:

.. code-block:: sql

   -- PostgresSQL
   WHERE user.is_admin::varchar like 'true'

Il est possible de saisir n'importe quelle chaîne de caractères dans
``$args[0]`` et pas seulement des valeurs booléennes.

.. attention::
   Cette recherche n'est pas sensible à la casse. Plusieurs fonctions de
   formatage sont appelées sur ``user.is_admin`` avant de tester l'égalité.

Tester si une donnée est présente ou non dans un groupe de données
..................................................................

Exemple: recherche des utilisateurs administrateurs.

Dans cet exemple, l'information se trouve non pas dans la table utilisateur mais
dans la table administrateur disposant d'une colonne ``user_id`` (clé
étrangère). Il nous faut utiliser une sous-requête pour récupérer l'ensemble des
identifiants de la table administrateur afin de tester si un identifiant
utilisateur est effectivement présent dans cette liste.

.. code-block:: php

   <?php

   // soit 'user' une table contenant pas la colonne 'is_admin'
   // soit 'admin' une table contenant une colonne 'user_id'

   $args = array();
   $args[0] = array('', 'true', 'false');
   $args[1] = array(_('Tous'),
                    _('Administrateurs'),
                    _('Utilisateurs simples'));

   $subquery = 'SELECT user_id FROM admin';

   $champs['administrator'] =
       array('colonne' => 'id',
             'table' => 'user',
             'libelle' => _('Administrateur'),
             'type' => 'select',
             'subtype' => 'manualselect',
             'where' => 'insubquery',
             'args' => $args,
             'subquery' => $subquery);

   ?>

Cette configuration permet de créer un champ HTML de type ``select`` avec
trois choix:

- Tous (valeur '');
- Administrateurs (valeur ``true``);
- Utilisateurs simples (valeur ``false``).

Le tableau ``$args[0]`` contient les valeurs associées aux choix. La valeur
``true`` indique que les identifiants des utilisateurs doivent se
trouver dans la sous-requête. La valeur ``false`` indique qu'ils ne
doivent pas se trouver dans la sous-requête. Contrairement à l'exemple
« Créer un champ de recherche avec menu déroulant personnalisé », les valeurs ne
seront pas recherchées telles quelles dans la base de données et ne doivent
surtout pas être modifiées.

En sélectionnant « Administrateurs », la requête SQL de recherche sera
construite comme suit:

.. code-block:: sql

   WHERE user.id IN (SELECT user_id FROM admin)

Rechercher une valeur dans une table liée
.........................................
Dans cet exemple, on recherche les contacts d'un établissement. Les contacts sont 
affichés dans un onglet de la table établissement.

La recherche porte sur le nom et le prénom des contacts.

Il faut ajouter à la requête standard la table de contacts puis paramétrer le critère
de recherche :

.. code-block:: php

   $table .= " LEFT JOIN ".DB_PREFIXE."contact
        ON contact.etablissement=etablissement.etablissement ";

Dans le code des critères de recherche : 

.. code-block:: php

   $champs['contact'] = array(
      'table' => 'contact',
      'colonne' => array('nom','prenom','courriel'),
      'type' => 'text',
      'libelle' => _('contact')
   );
