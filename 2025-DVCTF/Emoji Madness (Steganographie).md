> Un message codé. Rien de plus, rien de moins
> 
> Flag format : DVC{...}
> 
> +emoji_madness.txt

Après une recherche sur le web, avec : https://dawid.dev/sec/hiding-data-in-emoji-an-introduction-to-unicode-steganography 
```
import regex  # à installer avec: pip install regex

def decode_emoji_message(emoji_string):
    # Emoji to bit mapping
    mapping = {
        "⛳": "0",
        "👨‍💻": "1"}

    # Extract grapheme clusters (emoji as full units)
    emoji_list = regex.findall(r'\X', emoji_string)

    # Convert to bits
    bits = ''.join(mapping[c] for c in emoji_list if c in mapping)

    # Convert to characters
    message = ''
    for i in range(0, len(bits), 8):
        byte = bits[i:i+8]
        if len(byte) < 8:
            break
        message += chr(int(byte, 2))
   
    return message

# Lecture du fichier
with open("emoji_madness.txt", "r", encoding="utf-8") as f:
    emoji_data = f.read()

# Décodage
message = decode_emoji_message(emoji_data)
print(f"Message caché : {message}")
```

<details>
<summary>Flag</summary>

`DVC{3m0j1s_4r3_c00l_bUt_b1n4ry_15_k1ng}`

</details>
