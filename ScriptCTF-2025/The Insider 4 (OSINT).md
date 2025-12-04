> Good luck! Note: max flag limit is 6 for a reason, you should be able to get it in less than that. If not, open a ticket. Flag is case insensitive
> 
> https://github.com/scriptCTF/scriptCTF26/tree/main/OSINT/.insider-4 
> 
> With the elements : 
> Description: "As a photographer, I took these photos on my vacation. Flag format is scriptCTF{HOTEL_ADDRESS_ROOMNUMBER}. Example: scriptCTF{1337_elite_Hwy_S_9999} Have fun!"
>
> In attachments : fireworks.jpg & room.jpg.


In .secret : “as a photographer, i add comments/descriptions to my images” + exif for both, mention of Wendell Family Fireworks, google to find the location "Rockport, Texas".

Which hotel ? 
Recherche google image en zoomant sur la partie gauche + rockport texas hotel -> première photo pour https://www.travelweekly.com/Hotels/Rockport-TX/Days-Inn-by-Wyndham-Rockport-TX-p54377897 : Days Inn by Wyndham Rockport TX, 901 N Hwy 35, Rockport, TX 78382.

The website is https://www.wyndhamhotels.com/days-inn/rockport-texas/days-inn-rockport-texas/overview 

On the github page, it said “Flag format is scriptCTF{HOTEL_ADDRESS_ROOMNUMBER}. Example: scriptCTF{1337_elite_Hwy_S_9999}”

Donc 901_Hwy_35_N_…

Which room ? 
https://www.youtube.com/watch?v=GVgsfg5poNA 123, 121, 120, 119 sont les rooms sur le côté droit (y’en a au moins 8 sur ce coté) de la photo.

Donc Room between and 110 and 113-144-115 ? (car sur la photo, légèrement décalé vers la gauche). 
Tests : ni scriptCTF{901_Hwy_35_N_112}, ni scriptCTF{901_Hwy_35_N_113}, ni 114 (trop à gauches). c’est le 111 ! 

<details>
<summary>Flag</summary>

`scriptCTF{901_Hwy_35_N_111}`

</details>
