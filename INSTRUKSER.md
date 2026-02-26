# Frontend Februar instrukser

## Nyttige ressurser
Hvis man sitter fast eller sliter med en oppgave er dette en liste med gode ressurser:
- TypeScript docs: https://www.typescriptlang.org/
- Utility Types: https://www.typescriptlang.org/docs/handbook/utility-types.html
- Cheat Sheet: https://www.typescriptlang.org/cheatsheets/
- The TypeScript Course for JS Devs (YouTube): https://www.youtube.com/watch?v=K01hLNDdqg4
- Advanced TypeScript – Matt Pocock (YouTube playlist):https://www.youtube.com/playlist?list=PLIvujZeVDLMx040-j1W4WFs1BxuTGdI_b

Eller så kan man alltids spørre en venn eller LLM om hjelp

## Hvordan kjøre? 
Det går an å kjøre dette både lokalt (med feks vscode) eller i en playground på nettet. Playground er kjappest og enklest, men hvis man foretrekker å ha dette i det miljøet man er vant til så kan man kjøre det lokalt. 
### I playground i nettleser
 https://www.typescriptlang.org/play?install-plugin=%40type-challenges%2Fplayground-plugin

1. Installer plugin
Her får man spørsmål om å installere en plugin. Trykk Ja. Så vidt jeg forstod så er dette bare en plugin i ts sandkassa og ikke i nettleser e.l.

2. Velg oppgave
3. Løs oppgaven ved at det ikke er noen errors igjen i error-taben
Den vil starte med å se sånn ut (for oppgave 4):
![[Pasted image 20260226130608.png]]

og den er løst når den ser slik ut: 
![[Pasted image 20260226130651.png]]

### Klone og kjøre lokalt (i vscode)
NB: Krever: 
- Node.js
- `pnpm` 
- VS Code (eller annen IDE)

1. Klone prosjektet:

```bash
git clone git@github.com:vetlewj/frontend-ts-type-challenges.git
```

2. Installere avhengigheter
```bash

pnpm install

```

3. Generere playground filer:  
```bash

pnpm generate

```

4. Velg oppgave og åpne relevant fil i `playground` mappa.

Man vil få opp rød/oransje squiggly lines i vscode for typescript feil. Målet er da å fikse disse. Man kan også sjekke for feil ved: 

```bash
pnpm tsc --noEmit playground/<filnavn>
```
Dersom denne ikke printer noe i konsollen så har man fått til oppgaven. 



## Eksempel på oppgave og løsning: 4 — Pick

### 4 - Pick
by Anthony Fu (@antfu) #easy #union #built-in
### Question
Implement the built-in `Pick<T, K>` generic without using it.
Constructs a type by picking the set of properties `K` from `T`
For example:

```ts

interface Todo {

title: string

description: string

completed: boolean

}

  

type TodoPreview = MyPick<Todo, 'title' | 'completed'>

  

const todo: TodoPreview = {

title: 'Clean room',

completed: false,

}

```

```ts
/* _____________ Your Code Here _____________ */

  

type MyPick<T, K extends keyof T> = {

[key in K]: T[key]

}

  

/* _____________ Test Cases _____________ */

import type { Equal, Expect } from '@type-challenges/utils'

  

type cases = [

Expect<Equal<Expected1, MyPick<Todo, 'title'>>>,

Expect<Equal<Expected2, MyPick<Todo, 'title' | 'completed'>>>,

// @ts-expect-error

MyPick<Todo, 'title' | 'completed' | 'invalid'>,

]

  

interface Todo {

title: string

description: string

completed: boolean

}

  

interface Expected1 {

title: string

}

  

interface Expected2 {

title: string

completed: boolean

}

  

/* _____________ Further Steps _____________ */

/*

> Share your solutions: https://tsch.js.org/4/answer

> View solutions: https://tsch.js.org/4/solutions

> More Challenges: https://tsch.js.org

*/
```


### Løsning og forklaring
Her kan man feks endre fra: 
```ts
type MyPick<T, K> = any
  
```
til :
```ts
type MyPick<T, K extends keyof T> = {
	[key in K]: T[key]
}
```

Dette funker fordi: 
- `K extends keyof T` passer på at K er en gyldig nøkkel av T (title, description, completed for Todo eksempelet)
- `[key in K]` går gjennom hver valgte nøkkel
- `T[key]` kopierer value typen fra det originale objektet. ("Gi meg typen til propertien `[key]` inni T". Feks.: typen til property title fra Todo)**