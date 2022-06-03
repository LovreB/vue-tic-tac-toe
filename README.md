# 0. **Welcome and Run Template Project** 
Hello all Berliner Code Pubers! In this workshop, we will be creating a small Tic-Tac-Toe game in the JavaScript framework [Vue.js](https://vuejs.org/). 

![Tic tac toe in Vue.js](./base-done.png "tic tac toe board")

---
## 0.1 **How to read this Readme**
---
To easily follow this Workshop and Readme, the Readme is structured according to a few conventions. 

- Code and file names are written as `CodeExample`.
- Larger code snippets are writting in block, where the file name is included as a comment on the top, for example as 
```
// File Name

Newly added code together with some older code to understand where the new code should be put in
```
- If there are previous written code in the same file as the new code, this is implied by

```
/* ... */ (Previous code written)
```
- When Vuejs concepts or other important words are mentioned, they are written in bold, as **Vue Concept**

- If there are some deeper explainations to either concepts or why, these are written as the citation shown below. These can be skipped depending on how much in detail you want to go. 
> Example explaination of a concept

---
## 0.2 **Intro to the project**
---
If you are using an internal editor on your computer, you can fetch the basic project from the github repository [here](https://github.com/LovreB/vue-tic-tac-toe). Otherwise, just continue on here at [Vue.js playground](https://sfc.vuejs.org/). 

The template consists of three different Vue.js files - `App.vue`, `Board.vue` and `Square.vue`, where each file represents a **Vue component**. A Vue Component is a resuable block, which can contain other components or other html content. A Vue application contains one root Vue component, which in turns contains other **child components**. In our application, `App.vue` is the root component. 

![Vue Component tree](./components.png "A vue component tree example")

---
# 1. **Setting Up the Board**
The first task is to set up the board with nine squares, and fill a square with and `X` when clicked.  

Inspecting our root compoment `App.vue` file, there are three different parts. First is the template part, where the HTML template of the component is defined. If the compoment includes other components, they are added in a tag here, like `<Board />`. 

````
<template>
  <h2>HELLO CODE-PUBERS!</h2>
  <div class="board-container">
    <Board />
  </div>
</template>
````
A script part where the logic and data is added - here written in TypeScript. 
````
<script setup>
import Board from "./Board.vue"; // Imports the Board component so that it can be used in the template

</script>
````
And lastly, a style part with css. This workshop will not care about the styling - but feel free to modify if you want to. 
```
<style>
h2 {
  text-align: center;
}
.board-container {
  margin: auto;
  max-width: 138px;
  padding-top: 30px;
}
</style>
```

Running your app - either locally or checking the preview in Vue.js Playground - should display two texts - the Hello Codepubers heading, and The Board which is the Board component. 

![The starting point of project](./base-start.png "Start state of project")

---
## 1.1 **Add your first squares**
---
First step is to start fill the board with squares. To Recall the [component tree] - we are now adding 9 Square components as children to the Board component. 

In `Board.vue`, we want to add . We also need to *import* the Square to be able to use it in our Board component, which is done in the script part. 
```
// Board.vue
<template>
  <Square />
</template>
<script setup>
import Square from './Square.vue'
</script>
``` 
You should now be able to see a single square in the preview. 

But a board does not consist of one square, right? Since we want 9 squares we need a representation. In Vue, data models that can change are usually (TODO: LOOK UP STATEMENT). In the script, add 
```
const boardSquares = ref(Array(9).fill())
``` 
This creates a *reactive* variable, where the value, i.e. the array, later on can be used using `boardSquares.value`. Read more about reactive values HERE (TODO: FILL IN)

For each of these squares, we want to add an actual square. This is done via the vue rendering option (TODO: Check wording) `v-for`. In the template, replace `<Square />` with 

```
  <Square v-for="(value, index) in boardSquares" :key="index"/>

``` 
This will create one square for every square in `boardSquarares`, resulting with nine squares in the preview. 

## 1.2 **Filling the squares with Props**

In Vue.js, data bindings are an essential concept - data are being passed between components, and the template is re-rendered as soon as data is updated. When we pass data between parent - child, we do this via the components *props*.
To your `Square.vue`, add the 

```
<template>
  <span class="square">{{ value }}</span>
</template>
<script setup>
defineProps({
  value: String,
});
```
* Define props is used, and saying what type it is. Can then be used directly in the templated

Need to pass the value to component - which we are doing by binding the `square` to the `value` prop, by adding the attribute `:value="value"` in our v-for.

You should now see lots of `X` on the board.

## 1.2 **Only fill when clicked using Emits**
To avoid having a pre-filled board, we will now implement to only fill a square with `X` when it is clicked. 
TODO FILL IN TEXT
Add the method `updateBoard`

```
// Board.vue
<script setup>

/* ... */

const updateBoard = (i) => {
  board.value[i] = "X";
};

</script>
```
TODO: FIX CODEISH LANGUAGE. 
This method gets the array encapsulated by the reactive board variable, by calling the value property. n the board - ref  . Due to the reactiveness, all components that are using the board ref will re-render once a value like this is updated. 

But to call this method when a square is clicked, we need to add an *onClick event listener* to our `Square` components by using vues `v-on`, with shorthand `@`, directive. Add to the Square component
`@click=updateBoard(i)`. The updateBoard function is now called once we click a single square, and we are sending the index of the square we want to update. 

# 2. **Alternating players and compute winner**
Next step is to alternate players! 
## 2.1 **Alternate players**
To do this, we need a state keeping track on whos turn it is. Thus, add a new reactive variable in `Board.vue` by 
```
// Board.vue
<script setup>

/* ... */ 

const isNext = ref('X') 

/* ... */ 

</setup>
```
To use this variable, we need to modify our `updateBoard` function to both use our newly created ref value, and to toggle the player each time. 
```
// Board.vue
<script setup>

/* ... */ 

const updateBoard = (i) => {
  board.value[i] = isNext.value;
  isNext.value = isNext.value === "X" ? "O" : "X";
};
</setup>
```
Now you should se `X` and `O` alternatively being filled in the squares. 

## 2.2 **Compute winner**
But when does the game end? When someone gets three in a row! Add the helper function below to compute the winner in `Board.vue`

```
<script setup >
/** ... **/
const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
      return squares[a];
  }
  return null;
};
</script>
```
and in the `updateBoard` function, we need to calculate and check for the winner in every update. We also want to check if the square we are clicking already has a filled value - if so, we do not want to update the squares value or change the user. 
```
// Board.vue
<script setup>

/* ... */ 

const updateBoard = (i) => {
  const winner = calculateWinner(board.value);
  if (winner || board.value[i]) return;

  board.value[i] = isNext.value;
  isNext.value = isNext.value === "X" ? "O" : "X";
};
</setup>
```
You should now have a game that stops if anyone has won, and it is not possible to click a square that is already filled. 
# 3. **Displaying the winner by lifting state up**
The final step of the "Base" part is to display the winner. To do this, we want to alter the text displayed in our `App.vue`.


## 3.1 **Pre work - lifing state up**
To be able to use , we are going to "lift our state up" - i.e. move the logic from `Board.vue` to `App.vue`. Lets start with the `boardSquares` - move them from being a reacting variable in `Board.vue`, to `App.vue`, and send the array as prop down to the Board component. The prop is declared in `Board.vue` with `defineProps`. 

```
// Board.vue

<script setup>
import { ref } from "vue";
import Square from "./Square.vue";

const props = defineProps({
  board: {
    type: Array,
  },
});

/* ... */
</script>

// App.vue

<template>
  <h1>HELLO CODEPUBERS!</h1>
  <div class="container">
    <Board :board="board" />
  </div>
</template>
<script setup>
import { ref, computed } from "vue";
import Board from "./Board.vue";

const board = ref(Array(9).fill(null));

/* .. */ 
</script>
```
However, this should generate a warning as we are trying to update a prop from the child in our `updateBoard` function.

> In Vue.js, you should never update a prop from the child. This is always done through the parent which is passing the prop down. 

Next we want to lift all the logic up - `calculateWinner`, `updateBoard`, and `isNext` should be moved to `App.vue`. 

However, this will return a new error - "updateBoard does not exist". Since we are calling `updateBoard` from our template in `Board.vue`, we need to change that as well.

*There are different ways of handling this state. Another option would be to keep the board state on the board, and only emit if players have changed or have won. However, that would create muliple state-keeping, and adding future functionality, it would be most nice to have all game logic in the `App.vue` (or `Game.vue` if other stuff might be added to `App.vue`*

## 3.2 **Send events from child to parent using emit**

Instead of calling `updateBoard`, we want to tell our parent (The app component) that an action has taken place. If we only would use `@click` on the Board Component, we have no information about what square was used. Instead, we need another way. 

For communication from child to parent, vue has a concept called `emit` - and you are *emitting events* from the child to the parent, and listens to that events with the `v-on/@` directive at the parent. We call our event `squareClicked`, and we start with defining at the board that this is an event which can be emitted by using `defineEmits`, and changing the `@click`

```
// Board.vue
<template>
  <Square
    v-for="(value, index) in board"
    :key="index"
    :value="value"
    @click="emits('squareClicked', index)"
  />
</template>
<script setup>
/* ... */
const emits = defineEmits(["squareClicked"]);
</script>
```
In `@click="emits('squareClicked', index)"`, we are telling our component to emit `squareClicked`, with the payload `index` (so that the parent knows the index of the square which was clicked). 

## 3.2 **Add winner text with computed property**
Last step of the game is to print the name of the winner, or print whose turn it is next. We will replace `Hello Codpubers` text in `App.vue`. We will replace it with a so called *computed property*. A computed property in Vue.js is a value which depends on other variables, and is updated when they are updated. We will add the computed property to `App.vue` with the following code: 

```
// App.vue
const nextTurnText = computed(() => {
  const winner = calculateWinner(board);
  return winner
    ? `The amazing tic-tac-toe winner is: ${winner}`
    : `Next up is: ${isNext.value}`;
});
```
Here, we first check if anyone has won - if there is a winner, we add a text telling that is the winner. Otherwise, we print the value of `isNext` as that is who is next up. 

To display this in your template, change the h2-tag to display our `nextTurnText`

```
// App.vue
  <h2>{{ nextTurnText }}</h2>
```

Congratulations - you have now created your own tic-tac-toe game in Vue.js! To extend the application, you can now either select one of the extension proposed below, or add other features according to your choice (imagination has no limit :yay-frog:). If you don't know how to implement it, it is just to ask.

* Properties
* Emits
* Components
* Vue Directives
* Data Bindings

# 4. **Extensions** 

To further challenge yourself, you can select one (or multiple!) of the extensions below - they are not dependent on each other. 

## Extension A - Replay game and display user names
* TODO
- Add button with "New game" 
- Add field for typing names
- Display names instead of X
- Adapt model for player to include both name and value (X, O, V)

## Extension B - Style board depending on state
- Mark background / numbers on winning row
- need to return the index of the winning squares
- Also change X O to use different backgrounds instead

## **Extension C - Three-player-game**
The task of this assignment is to extend our game to be three-players playing on a 4x4 board, what will look sort of like: 

![Three player tic tac toe](./three-player.png "Three player tic tac toe")

### C.1 **Adjust players**


### C.2 **Adjust board**

- Use 4x4 board
- Fix algo
- Update user

## Extension D - Explore Life Cycle Hooks and Router
- Add an external "user settings" page on another site
- Select X O symbols
- use state for this and router link navbar. 
