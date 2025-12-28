> Ground Control to Space Cadet!
> 
> We've intercepted a satellite control program but can't crack the authentication sequence. The satellite is in an optimal transmission window and ready to accept commands. Your mission: Reverse engineer the binary and find the secret command to gain access to the satellite systems.
>
> \+ chall file

Avec dogbolt, on remarque que le code fourni montre un programme qui compare une entrée utilisateur avec un mot de passe de la forme formatée :
sprintf(&s, "sun{%s_%s_%s}\n", "e4sy", "s4t3ll1t3", "3131").
On concatène alors les éléments proprement.

<details>
<summary>Flag</summary>

`sun{e4sy_s4t3ll1t3_3131}`

</details>
