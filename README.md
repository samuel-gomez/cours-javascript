Sommaire

- Let/const
- Object
- The this Keyword
- Binding this
- Arrow Functions
- Arrow Functions and this
- Array.map Method
- Object Destructuring
- Spread Operator
- Classes
- Inheritance 
- Modules
- Named and Default Exports

## Let / const

Comme dans la plupart des langages, en Javascript, on peut utiliser la propriété **var** pour déclarer des variables.

```javascript
function sayHello() {
  for(var i = 0; i < 5; i++) {
    console.log(i);
  }
  console.log(i);
}
sayHello();
```

La portée de la variable est restreinte à la fonction, on ne peut accéder à "i" que dans la fonction.
Autre particularité, il est possible de réaffecter une valeur à tout moment dans la fonction.

Cela peut poser des problèmes, en principe, la déclaration d'une variable devrait se limiter à la portée du block dans laquelle, elle est déclarée.
Si l'on regarde l'exemple ci-dessus, le console.log situé en dehors de la boucle, affiche "5" alors que la boucle avait restreint la variable i à inférieur à 5. De plus, la variable n'est utile que pour la boucle.
Pour résoudre ce problème, ES6 a proposé une nouvelle propriété : **let**, qui permet limiter la portabilité au bloc.

Exemple : 

```javascript
function sayHello() {
  for(let i = 0; i < 5; i++) {
    console.log(i);
  }
  console.log(i);
}
sayHello();
```
On constate que la variable "i" n'est plus définie en dehors de la boucle.

Il est donc conseillé de plutôt utiliser **let** que **var** sauf pour un besoin spécifique.

Il existe une autre propriété ES6 pour déclarer des constantes : **const**
Cette propriété permet de définir des constantes qui ne pourront être modifiées, contrairement à **varv ou **let**.
Elle dispose du même scope que **let**.

Exemple :

```javascript
const x = 1;
x = 2;
```
On constate une erreur car on a tenté de réassigner une valeur à **x** qui est déclaré en **const**.

> **Remarque** : ceci n'est pas forcement vrai sur un objet, en effet, il est possible de réassigner une valeur sur un élément d'un objet : 


```javascript
const x = {
  title: "hello"
};
console.log(x);
x.title = "world";
console.log(x);
```

Par contre, cela va à l'encontre du principe d'immutabilité car on change la valeur de **x.title** même sur la premier console.log alors que l'on effectue la modification après, et cela peut créer des effets de bord.
Il est donc fortement déconseillé de faire cela.


## Object

Cette dernière remarque me permet faire une transition sur les objets Javascript.

La déclaration d'un objet se fait par clé/valeur : 

```javascript
const name = "name";
const objet = {
  cle: "valeur", // clé / valeur
  maFonction: function() {}, // on peut déclarer une fonction dans une clé
  maFonction2() {},  // on peut également déclarer directement la fonction, la clé sera le nom de la fonction
  name, // si la clé et la valeur ont le même nom, on peut utiliser une version raccourci => name: name 
};
```

## "this" keyword

## Binding this

## Arrow function

## Arrow function and this

## Array.map

## Object Destructuring

## Spread Operator

## Classes

## Inheritance

##



