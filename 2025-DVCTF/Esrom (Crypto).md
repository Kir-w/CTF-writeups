> Trouvez le message caché dans ce texte :
> 
> gLu4SLtAiLu4CIu0iLg0SLt0SLg0SLg4iLt0iLtAiLt4CIt0SLt0CIu4SLuAiLu0SLu0CIu4iLt0CIu4SLuAiLt0SLtAiLt4iL
> 
> morse.jpg
> 
> Flag format : DVC{message_cache}

Comme le titre est "Esrom", on essaie d'inverser la chaîne : 

```
s = "gLu4SLtAiLu4CIu0iLg0SLt0SLg0SLg4iLt0iLtAiLt4CIt0SLt0CIu4SLuAiLu0SLu0CIu4iLt0CIu4SLuAiLt0SLtAiLt4iL"
print(s[::-1])
```
ce qui donne: Li4tLiAtLS0tLiAuLS4uIC0tLi4uIC0uLS0uLiAuLS4uIC0tLS0tIC4tLiAtLi0tLi4gLS0gLS0tLS0gLi0uIC4uLiAtLS4uLg.

On décode en Base64 : 
```
import base64

code_base64 = "Li4tLiAtLS0tLiAuLS4uIC0tLi4uIC0uLS0uLiAuLS4uIC0tLS0tIC4tLiAtLi0tLi4gLS0gLS0tLS0gLi0uIC4uLiAtLS4uLg=="
decoded = base64.b64decode(code_base64).decode('utf-8')
print(decoded)
```
ce qui donne : 
..-. ----. .-.. --... -.--.. .-.. ----- .-. -.--.. -- ----- .-. ... --...

Puis dcode Morse (https://www.dcode.fr/code-morse) : 
```
s = "..-. ----. .-.. --... -.--.. .-.. ----- .-. -.--.. -- ----- .-. ... --..."

print(s[::-1])
```
ce qui donne : 
...-- ... .-. ----- -- ..--.- .-. ----- ..-. ..--.- ...-- ..-. .---- .-..

Puis avec dcode Morse : 3SR0M_R0F_3F1L
```
s = "3SR0M_R0F_3F1L"
print(s[::-1])
```

<details>
<summary>Flag</summary>

`DVC{L1F3_F0R_M0RS3}`

</details>
