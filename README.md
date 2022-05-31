# AgePredicter
![Exempel på hemsidan](https://github.com/AbbMyBjo/agePredicter/blob/main/website/src/assets/agePredicter%20exempel.png)

## Introduktion
### Problemformulering
I detta projekt har jag skapa en hemsida där jag anropar en API som med hjälp av användarens input, i detta fall ett namn, räknar ut hur gammal användaren är. Med denna hemsida kommer folk kunna ta reda på rolig information om sina namn och andras.

### Planering
Planen var att skapa ett Vue-projekt och ta bort alla onödiga features, de jag inte behöver. Efter det gör jag en enkel input och en ”send”-knapp där man får mata in sitt namn. När man klickar på ”send” anropas API:n med det inmatade namnet som värde, med en GET-request. API:n skickar tillbaka den uträknade åldern som jag visar på hemsidan i ett snyggt format.

### Arbetsprocess
Till att börja med gjorde jag en grundläggande kod för Agify API:n. Där anropades API:n med namnet som matades in i input, och den returnerade en ålder. När det var klart låg fokus på att få det snyggt. Efter att ha fått ett okej utseende på sidan fortsatte jag med resterande API:er och felhantering samt en "laddar"-indikator.

Efter att ha jobbat i några timmar med detta projekt bestämde jag mig för att utöka det en aning. Nu anropas tre API:er som returnerar ålder, könstillhörighet och en landskod för det land du väntas tillhöra. Allt detta baseras på det inmatade namnet. Då detta också blev klart fortare än tänkt ska jag nu anropa ytterligare en API, REST Countries, som jag använder för att göra om en landskod, som fås från Nationalize Api, till namnet på landet.

## Programmering
### Frontend
Språk som används: 
* HTML (https://sv.wikipedia.org/wiki/HTML)
* JavaScript (https://sv.wikipedia.org/wiki/Javascript)
* CSS (https://en.wikipedia.org/wiki/CSS)

Bibliotek som används:
* Vue (https://vuejs.org/)
* Axios (https://axios-http.com/docs/intro)

Till detta användes det JavaScript-baserade ramverket Vue, version 2.

### Backend
De API:er som anropas är:
* Agify (https://agify.io/)
* Genderize (https://genderize.io/)
* Nationalize (https://nationalize.io/)
* REST Countries (https://restcountries.com/)

### Starta hemsidan
Importera alla bibliotek som behövs och se till att ``npm install`` körts en gång. För att sedan starta projektet skrivs ``npm run serve`` i terminalen då man befinner sig i den path som gäller för "website", ``{mapp}\agePredicter\website``

## Avslutning
### Konsekvenser
Tack vare detta projekt kan människor på ett enkelt sätt söka upp information om sina namn och dela med sig av denna med sina vänner. Utöver sina egna namn kan de även söka upp kompisars eller andra personers namn av ren nyfikenhet. Detta kommer bidra till många roliga konversationer som i sin tur för personerna närmare varandra och ger dem glädje.

#### För vidare information se kommentarer i koden
