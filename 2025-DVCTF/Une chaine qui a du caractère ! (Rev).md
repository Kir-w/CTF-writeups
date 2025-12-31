> Je ne comprends pas c'est pourtant simple...
> 
> Flag format : DVC{motdepasse}
> 
> Ressources :
> https://dogbolt.org/
> 
> +fichier

Le programme copie la chaîne "ЅuреrЅесurе" dans local_148[0] jusqu'à local_148[11].
Et il place l'input utilisateur dans local_148[12] et au-delà.
Puis il compare les deux ; le mot de passe doit être identique à "ЅuреrЅесurе" pour que la comparaison fonctionne.
Et on remarque que la chaîne "ЅuреrЅесurе” est une version avec des lettres cyrilliques qui ressemblent à des lettres latines. Les caractères doivent être les bons Unicode.

<details>
<summary>Flag</summary>

`DVC{ЅuреrЅесurе}`

</details>
