Sommaire

- Let/const
- Object
- This & Binding
- Arrow Functions
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
Cette propriété permet de définir des constantes qui ne pourront être modifiées, constrairement à **varv ou **let**.
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

Par constre, cela va à l'enconstre du principe d'immutabilité car on change la valeur de **x.title** même sur la premier console.log alors que l'on effectue la modification après, et cela peut créer des effets de bord.
Il est donc fortement déconseillé de faire cela.


## Object

Cette dernière remarque me permet faire une transition sur les objets Javascript.

La déclaration d'un objet se fait par clé/valeur : 

```javascript
const name = "Guillaume";
const objet = {
  cle: "valeur", // clé / valeur
  maFonction: function() { console.log('ma fonction');}, // on peut déclarer une fonction dans une clé
  maFonction2() {},  // on peut également déclarer directement la fonction, la clé sera le nom de la fonction
  name, // si la clé et la valeur ont le même nom, on peut utiliser une version raccourci => name: name 
  email: "value"
};
```

Pour utiliser un objet, deux façons : 

```javascript
// dot notation (prefer this)
objet.maFonction();

// array notation
objet['maFonction']()
// 
```

La notation par tableau peut-être utile si on souhaite dynamiser la clé.

Par exemple : 
```javascript
// on récupère le nom d'un champ de formulaire donnée saisie par l'utilisateur
const inputFieldName = "email";
console.log(objet[inputFieldName])
// 
```

## This & Binding
**this** est mot-clé réservé en Javascript

Prenons en exemple ce code :
```javascript
const person = {
  name: "Tony",
  walk() {
    console.log(this);
  }
}
person.walk();
```
Ici, **this** représente le constexte de Person

Si on créé une référence de la fonction et on l'exécute : 

```javascript
const walk = person.walk; //notez qu'on n'exécute pas la fonction 
walk()
```
Cela s'explique par le fait que lorsque l'on appelle une function par son objet, this fait référence à cet objet.
Alors que si vous appelez une fonction en passan par objet autonome ou en dehors de l'objet, on perd la référence.
Les navigateurs feront alors référence à l'objet window ou undefined en stric mode.

Il est possible de corriger ce problème avec l'utilisation de la fonction bind : 

```javascript
const walk2 = person.walk.bind(person); // cela a pour effet de recréer une instance l'objet person 
walk2()
```

## Arrow function

C'est un nouvelle notation pour la déclaration de fonction en Javascript.

Voici l'ancienne façon de déclarer une fonction : 

```javascript
const addition = function(a,b) {
  return a+b;
}
const square = function(a) {
  return a*a;
}
```
En ES6 : 
```javascript
const addition2 = (a,b) => a+b;
const square2 = a => a * a;
```
Le premier avantage, c'est la simplication de la synthaxe.
Prenons un exemple plus concret : 
```javascript
const jobs = [
  { id: 1, isActive: true },
  { id: 2, isActive: true },
  { id: 3, isActive: false },
];
```
// on souhaite récupérer la list des jobs actifs, pour cela on va utiliser la fonction **filter** : 

```javascript
const activeJobs = jobs.filter(function(job){ return job.isActive;});
// avec une arrow fonction
const activeJobs2 = jobs.filter(job =>job.isActive);
```
Une autre particularité de l'arrow function est le bind.
Ajoutons à notre exemple un settimeout pour différer le log :

```javascript
const person = {
  name: "Tony",
  walk() {
    settimeout(function() {
      console.log('this : ',this);
    },1000)
  }
}
person.walk(); // on obtient le constexte window

const person2 = {
  name: "Tony",
  walk() {
    setTimeout(() => console.log('this : ',this),1000)
  }
}
person2.walk(); // on obtient le constexte de l'objet
```

## Array.map
Un fonction très utilisée pour parcourir un tableau, faire un traitement sur chaque item et renvoyer un tableau.

Exemple : on souhaite générer une liste HTLM à partie d'une liste de données : 

```javascript
const persons = ["Harry", "Peter", "mickael", "bob"];
const items = persons.map(person => `<li>${person}</li>`);
```
> On note ici l'utilisation du **template litteral** qui permet d'utiliser des variables dans une chaine de caractères sans faire de concaténation.

## Object Destructuring
Le Destructuring permet de simplifer la façon de récupérer des parties d'un objet.

Prenons l'exemple : 

```javascript
const address = {
  street: '10 rue de Lille',
  city: 'Paris',
  country: 'france',
  phone: '0101010101'
};
```
On souhaite récupérer chaque élément de l'objet dans des constantes : 

Méthode 1 :

```javascript
const street =  address.street;
const city =  address.city;
const country =  address.country;
```

Méthode 2 :
```javascript
const { street, city, country } = address;
```

> Dans la méthode 2, on ne choisit pas le nom des élements de l'objet mais on peut tout de même effectuer un renommage : 

```javascript
const { street: streetAddress, city, country } = address;
```

## Spread Operator
Une autre fonctionnalité qui sera utile pour faire du React.

Exemple : 

```javascript
const first = [1,2,3];
const second = [4,5,6];
```

Pour concaténer ces 2 tableaux, on peut faire : 

```javascript
const combined = first.concat(second);
```
On peut également le faire ainsi : 
```javascript
const combined2 = [...first,...second]; // on destructure chaque tableau pour en créer un nouveau
```
L'avantage de cette par rapport à l'autre est qu'elle apporte plus de souplesse : 
Par exemple, insérer des éléments entre les 2 tableaux : 

```javascript
const combined3 = [...first,'a',...second, 'b']; // on destructure chaque tableau pour en créer un nouveau
```
Autre avantage, il est très de faire une vraie copie d'un tableau : 

```javascript
const newFirst = [...first];
```
Le spread operator fonctionne également sur les objets : 

```javascript
const name = { name : 'John' };
const job = { job : 'Developer' };
const info = {...name, ...job };
```
On peut également surcharger une partie d'une objet (en restant immutable) : 

```javascript
const newJob = 'UX';
const updatedInfo = { ...info, job: newJob };
```


## Classes
Les classes en Javascript sont du sucre synthaxique, derrière il s'agit de simples objets.

Voici la structure classique : 

```javascript
class Person {
  constructor(name) {
    this.name = name; // ici on initialise une donnée interne avec la valeur passée à la création de l'instance
  }
  walk() {
    console.log(`${this.name} is walking`);
  }
}
```

On peut utiliser notre classe et créer une nouvelle instance :

```javascript
const peter = new Person('peter');
peter.walk();
```

## Inheritance

Il est possible d'étendre une classe à partir d'une autre.

```javascript
class Teacher extends Person {
  teach() {
    console.log('teach');
  }
}
```

On hérite de toutes les propriétés de Person et on peut utiliser les nouvelles propriétés.

```javascript
const sam = new Teacher('Sam');
sam.walk();
sam.teach();
```



