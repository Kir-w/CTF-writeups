> 50
> 
> Nous venons d’obtenir le DCP que Line Huks a confié pour les projections. Vous devrez décoder ce format et explorer son contenu pour découvrir ce qu'il dissimule. Avant de commencer, renseignez-vous sur le format DCP et ses particularités. Chaque détail pourrait être une clé pour résoudre cette énigme. Voyons ce qu’il cache.
>
> Compromis.zip

Dans le dossier, le fichier SousTitres.xml semble intéressant à analyser.
On repère ce sous-titre au timing suspect :

"<Subtitle>
<Start>00:00:48,500</Start>
  
<End>00:00:48,500</End>

<Text>
Dans Ce Pays, Hélas, malgré le fort potentiel qu'il A, C'est une période sombre : il subit un Krack Économique Dramatique.
</Text>
</Subtitle>"

En extrayant les majuscules du texte :
<details>
<summary>Flag</summary>

D C P H A C K É D

</details>
