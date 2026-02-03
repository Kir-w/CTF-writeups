> J'ai deux merveilleux chats chez moi : Mona et Lisa ! J'aurais bien aimé vous montrer à quoi elles ressemblent, mais elles sont très malines et ont touché à mon ordi pendant que je regardais ailleurs. Résultat : les fichiers ne s'ouvrent plus ! Pouvez-vous les restaurer pour moi ?
> 
> +fichier .zip

Avec xxd lisa.jpg | head et xxd mona.mp4 | head, on obtient respectivement : 
```
00000000: c47c 47c4 0010 4a46 4946 0001 0101 0060  .|G...JFIF.....`
00000010: 0060 0000 ffe1 028e 4578 6966 0000 4d4d  .`......Exif..MM
…
```
et
```
00000000: ffd8 ffe0 0010 4a46 4946 0001 0101 0060  ......JFIF.....`
00000010: 0060 0000 ffe1 028e 4578 6966 0000 4d4d  .`......Exif..MM
…
```

Pour lisa.jpg, on n’a pas une entête JPEG valide. Note : une image JPEG commence toujours par : FF D8 FF. Or ici, on a : C4 7C 47 C4.

Pour mona.mp4, on a FF D8 FF E0, ce qui est bien un début d’image JPEG (au lieu d'être un mp4).

Conclusion : les fichiers sont échangés.

On renomme les fichiers :
```
mv lisa.jpg lisa.mp4
mv mona.mp4 mona.jpg
```

et dans mona.jpg on voit : 
```
DVC{c4T5_4nD_NvMb3R5_
```

Pour lisa.mp4, il y a une autre manipulation à faire.

Hypothèse : les 4 premiers octets ont été modifiés.

Il est probable que le fichier soit un JPEG valide, mais avec une entête corrompue volontairement.

Dans https://hexed.it/, on remplace les 4 premiers octets : C4 7C 47 C4 par FF D8 FF E0.

On ouvre le fichier, et on lit alors la seconde partie du flag : 
```
4r3_tRuLy_m4G1c4L!}
```

<details>
<summary>Flag</summary>

`DVC{c4T5_4nD_NvMb3R5_4r3_tRuLy_m4G1c4L!}`

</details>
