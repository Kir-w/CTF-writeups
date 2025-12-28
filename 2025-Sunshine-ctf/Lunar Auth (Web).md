> Infiltrate the LunarAuth admin panel and gain access to the super secret FLAG artifact !
> 
> https://comet.sunshinectf.games

On ajoute /admin à la fin de l'URL, puis on inspecte la page.
On voit l'username (Base64 décodé) :
atob("YWxpbXVoYW1tYWRzZWN1cmVk") -> "alimuhammadsecured"

Et le password (Base64 décodé) :
atob("UzNjdXI0X1BAJCR3MFJEIQ==") -> à décoder.

Avec python
```
import base64
print(base64.b64decode("UzNjdXI0X1BAJCR3MFJEIQ=="))
```
Cela donne (en bytes) : b'S3cur4_P@$$w0RD!'

Donc les identifiants pour accéder à /admin sont :

Username : alimuhammadsecured
Password : S3cur4_P@$$w0RD!
 
<details>
<summary>Flag</summary>

`sun{cl1ent_s1d3_auth_1s_N3V3R_a_g00d_1d3A_983765367890393232}`

</details>
