# Module Drupal8 tbh_csv

## Description :
Ce module Drupal8 permet de faire la démonstration d'une migration avec un fichier de données CSV.
L'installation du module ne fait aucune autre action que celle d'enregistrer la configuration en base.
Pour effectuer les migrations, on utilise ensuite la ligne de commande avec __Drush__ (Drupal Shell)

## Voir aussi :
 - https://www.drupal.org/node/2574707

## Prérequis :
### 1) avoir installé les modules suivants
   - tbh_csv (ce module)
   - migrate_plus
   - migrate_tools

### 2) avoir installé Drush (version >8)
__Drush__ est en effet nécessaire pour faire les migrations, qu'on lancera via la ligne de commande.


# Paramétrages / Configuration :
Le fichier de configuration (à paramétrer) est
> tbh_csv\config\install\migrate_plus.migration.migrate_csv.yml

##1) Fichier de données
Le fichier contenant les données au format CSV doit par défaut être situé dans :
  public://csv/people.csv
Ce qui correspond au dossier :
  sites\default\files\csv\people.csv

Dans le fichier de configuration, la ligne
  path: public://csv/people.csv
indique quel doit être le fichier de données CSV

Il vous est ainsi possible de changer ce fichier ainsi que son emplacement

##2) Identifiant de la migration
La première ligne du fichier de configuration (yml), est la suivante :
id: csv_file_people
Cet identifiant est celui à utiliser lors de la commande Drush de migration.
Lors de la migration en ligne de commande, on fera donc :
> drush migrate:import csv_file_people

Il vous est ainsi possible de changer cet identifiant.


# Commandes de migration (avec Drush)
Les commandes Drush utiles sont les suivantes :
* Pour voir le statut des migrations :
```shell
    drush status
```
* Pour lancer la migration souhaitée (qui correspond à l'identifiant situé ds la configuration YAML) :
```shell
    drush migrate:import nom_migration
```
* Supprimer les données d'une migration :
```shell
    drush migrate:rollback
```



# Exemples de fichiers CSV
## Fichier people.csv
```csv
id,first_name,last_name,email,country,ip_address
1,Justin,Dean,jdean0@example.com,Indonesia,60.242.130.40
2,Joan,Jordan,jjordan1@example.com,Thailand,137.230.209.171
```
## Fichier article.csv
```csv
Id2,title,body
1,title 1,some body text 1
2,title 2,some body text 2
3,title 3,some body text 3
```

# Problèmes potentiels / Troubleshooting
Lorsque vous lancez la commande
```shell
   drush migrate:status
```
Si l'erreur suivante apparaît :
```shell
Fatal error: Unsupported operand types in C:\wamp\www\drupal8\core\modules\migrate\src\Plugin\Migration.php on line 665
```
Corrigez le fichier drupal8\core\modules\migrate\src\Plugin\Migration.php
en remplaçant la fonction getMigrationDependencies() par le code suivant

```php
public function getMigrationDependencies() {
  // ANCIENNE ligne :
  // return $this->migration_dependencies + array('required' => [], 'optional' => []);

  // Trois NOUVELLES lignes :
  $this->migration_dependencies['required'] = [];
  $this->migration_dependencies['optional'] = [];
  return $this->migration_dependencies;
}
```
# Credits

This project would not be possible without the following:

  * [Drupal.org](http://www.drupal.org/)

# Documentation License

[Creative Commons Attribution-NonCommercial-ShareAlike 3.0](http://creativecommons.org/licenses/by-nc-sa/3.0/)


# Code License

[MIT License](http://www.opensource.org/licenses/mit-license.php)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
