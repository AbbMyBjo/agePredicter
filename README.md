# AgePredicter

I detta projekt ska jag skapa en hemsida där jag anropar en API som med hjälp av användarens input, i detta fall ett namn, räknar ut hur gammal användaren är. Till projektet använder jag ramverket Vue.

Planen är att skapa ett Vue projekt och ta bort alla onödiga features, de jag inte behöver. Efter det gör jag en enkel input och en ”send”-knapp där man får mata in sitt namn. När man klickar på ”send” anropas API:n med det inmatade namnet som värde, med en GET-request. API:n skickar tillbaka den uträknade åldern som jag visar på hemsidan i ett snyggt format.

Efter att ha jobbat i några timmar med detta projekt bestämde jag mig för att utöka det en aning. Nu anropas tre API:er som returnerar ålder, könstillhörighet och en landskod för det land du väntas tillhöra. Allt detta baseras på det inmatade namnet.

De API:er som anropas är:
* Agify (https://agify.io/)
* Genderize (https://genderize.io/)
* Nationalize (https://nationalize.io/)

Till att börja med gjorde jag en grundläggande kod för Agify API:n. Där anropades API:n med namnet som matades in i input, och den returnerade en ålder. 
