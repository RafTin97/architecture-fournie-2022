# Infrastructure AWS - Socle de sécurité COJI

Ce code Terraform permet d'automatiser la création du socle de sécurité de l'environnnement Cloud COJI.
Les différentes règles de la PSSI du COJI ont été mises en place.

Prérequis :
- Installer Terraform
- Installer la CLI AWS
- Configurer son provider
- Configurer l'id de son bucket SCP

Remarques : 
- Attention à contrôler votre budget, des alertes ont été mises en place pour bloquer le compte en cas de dépassement
- N'oubliez pas de "détruire" vos ressources lorsque vous ne travaillez pas pour réduire les coûts

Tips : 
- Un accès console peut-être récupéré en créant le mot de passe de votre compte BlueWaver
  `aws iam create-login-profile --user-name BlueWaver --password <new-password> --profil <profile>`
- Il peut-être utile de configurer un remote backend terraform pour travailler en groupe avec un tfstate partagé

Commandes utiles :
|                   |                                        |
|-------------------|----------------------------------------|
| terraform apply   | Création des ressources                |
| terraform destroy | Destruction des ressources             |
| aws configure     | Configuration du profil aws par défaut |

## Utilisation des SCP

Afin de créer des SCP, vous pouvez modifiez les policies dans les fichiers scp-1.json, scp2.json et scp3.json.

Prérequis : 
- Remplir le nom de son bucket SCP dans le champ correspondant du fichier terraform.tfvars
  Le nom du bucket peut-être récupéré avec la commande suivante :
  `aws s3api list-buckets --profil <profile>`

Remarques : 
- Seules 3 SCP sont autorisées par compte et le bucket ne devra pas contenir plus d’objets. Vous pouvez cependant définir plusieurs « statements » dans une même stratégie. Attention, la taille maximale d'une SCP reste limitée à 5000 Bytes.
- Veuillez ne pas changer le nom des objets et des fichiers
- Attention, vous ne pourrez pas voir si vos SCP ont été correctement déployées. 
  Il faudra donc faire bien attention à ce que la syntaxe de la stratégie soit correcte. Le script "validate_policy.py" est mis à disposition afin de vérifier leur validité.
  Vous pourrez seulement vérifier le déploiement de vos SCP en testant leurs règles manuellement (en essayant de créer une ressource interdite par exemple)# architecture-fournie-2022
