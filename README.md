# AgePredicter

# Introduktion
I detta projekt har jag skapa en hemsida där jag anropar en API som med hjälp av användarens input, i detta fall ett namn, räknar ut hur gammal användaren är. Till projektet använder jag ramverket Vue.

Planen var att skapa ett Vue-projekt och ta bort alla onödiga features, de jag inte behöver. Efter det gör jag en enkel input och en ”send”-knapp där man får mata in sitt namn. När man klickar på ”send” anropas API:n med det inmatade namnet som värde, med en GET-request. API:n skickar tillbaka den uträknade åldern som jag visar på hemsidan i ett snyggt format.

Efter att ha jobbat i några timmar med detta projekt bestämde jag mig för att utöka det en aning. Nu anropas tre API:er som returnerar ålder, könstillhörighet och en landskod för det land du väntas tillhöra. Allt detta baseras på det inmatade namnet. Då detta också blev klart fortare än tänkt ska jag nu anropa ytterligare en API, REST Countries, som jag använder för att göra om en landskod, som fås från Nationalize Api, till namnet på landet.

Till att börja med gjorde jag en grundläggande kod för Agify API:n. Där anropades API:n med namnet som matades in i input, och den returnerade en ålder. När det var klart låg fokus på att få det snyggt. Efter att ha fått ett okej utseende på sidan fortsatte jag med resterande API:er och felhantering samt en "laddar"-indikator.

# Frontend
Språk som används: 
* HTML
* JavaScript
* CSS. 

Bibliotek som används:
* Vue 
* Axios

Till detta användes det JavaScript-baserade ramverket Vue, version 2.

# Backend
De API:er som anropas är:
* Agify (https://agify.io/)
* Genderize (https://genderize.io/)
* Nationalize (https://nationalize.io/)
* REST Countries (https://restcountries.com/)
