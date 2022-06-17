## Nous abortderon les HOOR 
Les conction d'utilisation des hook 
- [x] A l'interieur du composant 
- [x] A la racine du composant 
### UseState: Hook d'état : 
Il renvoie une **valeur d'état** et une fonction pour mettre ajour l'état actuel du composant fronctionnel 
(permet de préserver les valeur entre les appelle des fonction au sains d'u composant fonctionnel)
#### Syntaxe:
`const[value,setValue] = useState("some Text")` et revoie le valeur et la fonction qui permet de la modifier 
### UseEffect: Hook d'effet 
Il permet d'applicaquer **l'éffet de bord** lorsque l'état locale du composant fonctionnel à été modifié.
#### Syntaxe:
`UseEffecte(fns, arg)` **fonction de rappelle pour l'effer**  et une **liste de dépendance**, 
`UseEffecte(() => {
// do something here 
}, [dependance ])
` il s'execute une fois au momoent du chargement du composant === ComopomentDidMonte

### UseRef 
Il permet de faire de la programation inpérative sur un objet du dome, l'application de ce Hook permet d'avoir un **proporier current**
- on va pouvoir accéder au noeud du DOM et modifier des valeur de maniere impérative 
qui permet d'avoire acces a la valeur de l'élement du DOM

### UseReducer 
Donne une solution plus avancé de useState pour gérer une logique d'état plus complexe 
[il permet d'optimiser et rendre la lisibile la gestion de votre état ]
#### Syntaxe :
`const[state, dipatch] = useReducer(reducer, initialArg)`;
on a deux entrer :
1- un fonction reducer pout gere separement un Etat
2- les valeur initiale qui seron déclater commet dans useStat
et UseReducer renvoie le State (qui contion les valeur et les propriete de notre Etat) et  
Dispath (qui permet de sistribuer les actions qui vont modifer l'état pour etre récuper dans Stat)

### UseContext 
qui prent un objet en context pour renvoyer les valeur globale du context 
[evite la dublication de code et permet le partarge de logique de comportement entre composant]

### UseMemo 
permet de renvoie une **valeur mémoïsée** qui change uniquement si la valeur d'un des entrée à changé 
(utilie pour mémoriser des calcule et mettre en cage des opération pou éviter de calcule unitille)===> [shouldCompomenteUpdate]
#### Syntaxe 
il prend deux enter :
`const[valeu,setValue] = useState("some value")`
etrourne la valeur mémoïsée s'il y a aucun changement dans la liste de dépendans
`const ValueMemo = UseMemo(() => {
return{ value}
},[list de dependance])`

### UseCallback 
Permet de returner un fonction de rappel mémoïsée qui changera si la valeur d'une des entréea changé
#### Syntaxe 
const functionCallback = useCallback(() => {
dosomething(value)},[liste de dépendance ])

