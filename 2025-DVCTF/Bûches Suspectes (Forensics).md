> Le serveur web de la communauté de castors a été compromis en juillet. Un intrus a tenté de voler les précieux plans de construction du barrage. Père Castor vous demande alors d'analyser les logs du serveur Apache pour comprendre ce qu'il s'est passé.
> 
> +fichier.log

L'adresse IP 203.0.113.42 a tenté plusieurs injections SQL :
```
GET /products.php?id=1' OR 1=1-- 
GET /products.php?id=1' UNION SELECT 1,2,3-- 
GET /products.php?id=1' UNION SELECT user(),database(),version()-- 
GET /products.php?id=1' UNION SELECT column_name,2,3 FROM information_schema.columns WHERE table_name='users'-- 
GET /products.php?id=1' UNION SELECT username,password,email FROM users--
```
Certains paramètres semblent encodés en Base64, possiblement pour masquer des extractions de données :
```
RFZDVEZ7d3IwbmdfZkw0R19mMFJtNFR9

RFZDe0NsNHM1IWNfTDBnXzRONGxZNTFzfQ==

RFZDVEZ7aDNsbDBfQXl3ZXRofQ==
```
Avec dcode pour décoder en base64 : 
Le premier donne un leurre.

Le second donne : RFZDe0NsNHM1IWNfTDBnXzRONGxZNTFzfQ==

<details>
<summary>Flag</summary>

`DVC{Cl4s5!c_L0g_4N4lY51s}`

</details>
