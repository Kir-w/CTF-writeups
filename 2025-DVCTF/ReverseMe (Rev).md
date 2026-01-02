> Trouve le mot de passe de ce binaire.
> 
> +fichier ReverseMe 

Avec Ghidra, extraire : 

```
buf1 = [
    0xd26d24a10b6e8b80,
    0x6377d4e9ce576ade,
    0xf3807bb7231d9ac6,
    0x1cf661b7b2674d61,
]
buf2 = [
    0x8d1814f8702dddc4,
    0x3c449c9d9164189f,
    0x85e529e8774eff84,
    0x61d740c5d7143f52,
]
```
Et avec par exemple : 
```
# Conversion en bytes (little endian pour convention Ghidra / C)
buf1_bytes = b''.join([x.to_bytes(8, 'little') for x in buf1])
buf2_bytes = b''.join([x.to_bytes(8, 'little') for x in buf2])

flag = bytes([a ^ b for a, b in zip(buf1_bytes, buf2_bytes)])

print(flag.decode(errors='replace'))
print(flag.hex()) # affichage hex
```

<details>
<summary>Flag</summary>

`DVC{Y0u_Ar3_tH3_BeST_Rev3rser!!}`

</details>
