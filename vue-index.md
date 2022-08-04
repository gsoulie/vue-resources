# Ressources Vue3

* [Bases](https://github.com/gsoulie/vue-resources/blob/main/vue-basics.md)      


## Présentation

https://www.jesuisundev.com/comprendre-vuejs-en-5-minutes/      

VueJS est un framework Javascript frontend pour créer des interfaces utilisateurs. Tu vas me dire « encore un ? » et la réponse est oui. Sauf qu’il est un peu différent.

Déjà, c’est intéressant de comprendre que VueJS a été conçu pour être intégré de façon incrémentale. Ça veut dire que **si tu as une application frontend déjà existante, t’as pas à tout refaire. Tu peux faire une nouvelle partie en VueJS et l’intégrer rapidement au reste**.

VueJS prend aussi les bonnes idées de ses concurrents. Il permet le data binding. Les données et le DOM sont couplés et réactifs aux changements. On retrouve également le concept de virtual dom avec VueJS. Le DOM n’est pas directement changé, ça passe par le virtual DOM.

On retrouve aussi l’organisation par composant. Cette fonctionnalité permet de découper ton application en plusieurs sous-composants qui gèrent chacun leur vie et qui sont réutilisables. Imaginons tu veux faire une liste d’images : tu peux faire un composant qui gère l’image et un composant qui gère une liste de composant image.

VueJS se concentre sur la partie vue de ton application. Pour ce faire, le framework s’inspire en partie du patron d’architecture MVVM. VueJS va lier ton DOM, la partie vue, avec ton instance de vue, la partie Vue-Modèle. Ces deux parties sont liées par le système de data-binding
