> As a thank you for playing our CTF, we're giving out participation certificates! Each one comes with a custom flag, but I bet you can't get the flag belonging to Eth007!
> 
> https://eth007.me/cert/

Checking the code source first. Le site génère un flag au format ictf{hash} où 'hash' est une fonction de hash personnalisée (participant name).

La génération de ce hash se fait dans la fonction JavaScript customHash dans la page :
```
function customHash(str){
  let h = 1337;
  for (let i=0; i<str.length; i++){
    h = (h * 31 + str.charCodeAt(i)) ^ (h >>> 7);
    h = h >>> 0;
  }
  return h.toString(16);
}
```
Le flag est inséré dans la balise <desc> du SVG. Pour récupérer le flag pour le participant Eth007, on calcule localement ce hash pour le nom "Eth007" avec la fonction donnée en JS puis on compose le flag avec la syntaxe ictf{hash}.

En Python : 
```
def custom_hash(s):
    h = 1337
    for c in s:
        h = (h * 31 + ord(c)) ^ (h >> 7)
        h = h & 0xFFFFFFFF  # for unsigned 32-bit int
    return format(h, 'x')

name = "Eth007"
flag = f"ictf{{{custom_hash(name)}}}"
print(flag)
```

<details>
<summary>Flag</summary>

`ictf{7b4b3965}`

</details>
