# Lando starters
Lando simplifie l'utilisation de **Docker** et propose notamment des environnements préconfigurés pour *Wordpress* ou *Drupal 7 & 8*. Sa configuration est basée sur un simple fichier **.lando.yml** .

### Avant de commencer :

 - Téléchargement : https://github.com/lando/lando/releases
 - Documentation : https://docs.devwithlando.io/
 - Exemples d'utilisation : https://github.com/lando/lando/tree/master/examples
 - Docker : https://docker.io

### Fonctionnalités des fichiers starters

**De base :**
- Environnement Linux
- NPM
- Node (dont yarn)
- PHP
- Mariadb
- Apache
- SSL
- SSH
- Drush (Drupal uniquement)

**Custom :** 
 - Gulp
 - Mailhog (mail catcher)
 - Phpmyadmin
 - [Deployer.php](https://deployer.org/) 

## Utilisation

###  Fichier de configuration .lando.yml 

Les starters sont eux-mêmes basés sur les fichiers de configuration par défaut proposés par lando.
Dupliquer un des starters en *.lando.yml*, le placer à la base du site et le modifier :


| Donnée | Instructions | Exemples |
|--|--|--|
| *APPNAME* | Nom machine de l'application | mon-site

### Création des containers

Se placer en shell dans le dossier où est présent votre fichier *.lando.yml* et exécuter  `cd mypath/mon-site && lando start`

### Modification de la configuration

Pour tout ajout, suppression ou modification dans le fichier *.lando.yml*, un simple `lando restart` peut suffire.
Sauf (il faut bien une exception) pour les versions de bases de données mysql ou mariadb. Il convient alors d'exécuter `lando rebuild`

**/!\ :** `lando rebuild`, tout comme `lando destroy`, supprimera le contenu des containers, dont vos bases de données. Pensez à exécuter un `lando db-export [FILENAME]`

## Accès

Une fois les containers créés, un message apparaît simplement :

``` 
 BOOMSHAKALAKA!!!

 Your app has started up correctly.
 Here are some vitals:

 NAME            mon-site                                                
 LOCATION        /MYPATH/Sites/mon-site 
 SERVICES        appserver, database, node, mailhog, pma                        
                                                                            
 APPSERVER URLS  https://localhost:32857                                        
                 http://localhost:32858                                         
                 http://mon-site.lndo.site                               
                 https://mon-site.lndo.site                              
                                                                                             
 MAILHOG URLS    http://localhost:32856                                         
                 http://mail.mon-site.lndo.site                          
                 https://mail.mon-site.lndo.site                         
                                                                              
 PMA URLS        http://localhost:32855                                         
                 http://pma.mon-site.lndo.site                           
                 https://pma.mon-site.lndo.site    
```
Pas besoin de plus.

## Commandes shell

`lando --help` est votre ami.

Nul besoin de se connecter en ssh au container pour exécuter une commande, il suffit de préfixer la commande par lando.

Quelques commandes utiles  (non exhaustif) :



|Commande | Description |
|--|--|
| composer                | Run composer commands |
| config                  | Display the lando configuration |
| dep                     | Deploy app with deploy.php |
| gulp                    | Run gulp commands |
| logs [appname]          | Get logs for app in current directory or [appname] |
| restart [appname]       | Restarts app in current directory or [appname] |
| ssh [appname] [service] | SSH into [service] in current app directory or [appname] |
| start [appname]         | Start app in current directory or [appname] |
| stop [appname]          | Stops app in current directory or [appname] |
| yarn                    | Run yarn commands |

## Extras :

### Connexion aux bases de données :

Les données de connexion peuvent être récupérées automatiquement via les variables php d'environnement.

Par exemple pour l'utilisation  d'un recipe Wordpress (en commentaire les valeurs générées par lando) :

```
$secrets['database_host'] = getenv('DB_HOST'); // database
$secrets['database_name'] = getenv('DB_NAME'); // wordpress
$secrets['database_user'] = getenv('DB_USER'); // wordpress
$secrets['database_pass'] = getenv('DB_PASSWORD'); // wordpress
```
## Protips

### SSH keys missing

```
$ lando ssh
$ cd ~/.ssh
$ ln -s /user/.ssh/* .
```

### Install htop

Login as root user
```
$ lando ssh --user root
```

Once connected :
```
# apt-get update
# apt-get install htop
```

Run command :

```
$ htop // from appserver container
$ lando ssh --command "htop" // from your computer shell
```