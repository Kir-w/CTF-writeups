> The space base is in danger and we lost the key to get in!
> 
> \+ a binary file

Avec dobolt, on analyse le fichier binaire.

sub_4012c6 est un encodeur Base64. Le programme lit une entrée de 22 octets (strlen == 0x16) ; il calcule rax_5 = base64(input) puis compare : les 19 premiers caractères de rax_5 avec la chaîne littérale "c3Vue2MwdjNyMW5nX3V", puis les 13 caractères suivants (19..31) avec la chaîne littérale "yX0I0NTM1fQ==".

Autrement dit, il exige que base64(input) == "c3Vue2MwdjNyMW5nX3VyX0I0NTM1fQ==".

Donc pour retrouver l’input (i.e. le flag), on décode en Base64 la concaténation des deux morceaux.

```
import base64
s = "c3Vue2MwdjNyMW5nX3VyX0I0NTM1fQ=="
print(base64.b64decode(s).decode())
```

<details>
<summary>Flag</summary>

`sun{c0v3r1ng_ur_B4535}`

</details>
