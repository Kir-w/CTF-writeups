> Merde, mon réveil est encore cassé... Qu'est ce que j'ai bien pu faire pour qu'il m'affiche toujours ce message bizzare ?
> 
> img.png : “dUC-2025”
> 
> Je vais devoir retrouver les bits d'entrées de chaque affichage 7-segments qui permettent d'afficher ce message !
> Je me souvient que ces affichages utilisent des anodes, je dois avoir la doc quelque part.
> 
> Format du flag : DVC{BinaryLED1_BinaryLED2_...}
> Par exemple : DVC{1111111_1111111_1111111_1111111_1111111_1111111_1111111_1111111} si toutes les entrées sont à 1.

A la main avec https://si.blaisepascal.fr/1t-afficheur-7-segments/ : 
DVC{0111101_0111110_1001110_0000001_1101101_1111110_1101101_1011011}, mais non correct

Avec Dcode https://www.dcode.fr/afficheur-7-segments : 
0100001 1000001 1000110 0111111 0100100 1000000 0100100 0010010

<details>
<summary>Flag</summary>

`DVC{0100001_1000001_1000110_0111111_0100100_1000000_0100100_0010010}`

</details>
