> Vous êtes passionné·e par les mystères de Léonard de Vinci, et votre rêve est de voir La Joconde de vos propres yeux… en VIP, bien sûr.
> 
> Mais le billet pour cette visite privée exclusive coûte beaucoup trop cher. Trouver un moyen d'obtenir un billet.
> 
> Ressources :
> 
> Protocole HTTP et méthodes
> Les Bases de Burp Suite - Guide
> http://la-joconde-a-tout-prix.chall.davincicode.fr/

Première piste avec l’indication de Burp Suite est de manipuler les requêtes HTTP entre client et serveur (lors de l’achat du billet).

On configure Google Chrome afin d’utiliser Burp Suite en proxy sous Windows.

Dans Chrome, menu > Paramètres > Avancé > Système > Ouvrir les paramètres proxy

Dans la section Serveur proxy, on active "Utiliser un serveur proxy".

Pour adresse, on met 127.0.0.1 (localhost).

Pour port, on met 8080 (port par défaut de Burp Suite).

On décoche la case "Ne pas utiliser le serveur proxy pour les adresses locales" si elle est cochée.

Donc le navigateur, on envoie les requêtes HTTP vers l’adresse du proxy (habituellement 127.0.0.1:8080).

Burp Suite écoute à cette adresse et intercepte ces requêtes, te permettant de les examiner, modifier ou rejouer.

Dans Burp, sous l’onglet Proxy > Options > Proxy Listeners, on s'assure que l’écoute est sur 127.0.0.1:8080.

Sous Proxy > Intercept, on active l'interception pour voir/modifier les requêtes.

Avec Burp Suite en écoute, on navigue ensuite dans Chrome vers l’URL du challenge.

Dans Burp, dans l’onglet HTTP history ou Intercept, on voit toutes les requêtes HTTP faites par le navigateur.

Il nous reste à trouver la requête du montant restant dans le portefeuiille.

On envoie cette requête dans Repeater pour la modifier, on change la valeur dans Repeater, de 1 vers 9999, avant de la renvoyer au serveur.

<img width="451" height="236" alt="image" src="https://github.com/user-attachments/assets/fa8b7717-0280-43c1-b68a-4f45d9cf99a3" />

<img width="449" height="233" alt="image" src="https://github.com/user-attachments/assets/570a734d-dd0d-4c60-9962-bd4e2c136d7e" />

https://pylingual.io/ 

Dans la réponse, on obtient le flag.

<details>
<summary>Flag</summary>

`DVC{m0n4_l1s4_v1p_t1ck3t_h4ck3d_w1th_burp_su1t3}`

</details>
