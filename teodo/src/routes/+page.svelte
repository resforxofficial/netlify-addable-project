<script>
  /**
   * @typedef {{ text: string, liked: boolean, completed: boolean }} TodoItem
   */

  /** @type {TodoItem[]} */ let todoList = [];

  if (typeof localStorage !== "undefined") {
    const saved = localStorage.getItem("todoList");
    if (saved) {
      try {
        todoList = JSON.parse(saved);
      } catch (e) {
        console.error("todoList parsing error:", e);
      }
    }
  }

  let inputValue = "";

  const addTodo = () => {
    if (inputValue.trim() !== "") {
      /** @type {TodoItem} */
      const newTodo = {
        text: inputValue,
        liked: false,
        completed: false,
      };
      todoList = [...todoList, newTodo];
      inputValue = "";
      saveToLocalStorage();
    }
  };

  const saveToLocalStorage = () => {
    if (typeof localStorage !== "undefined") {
      localStorage.setItem("todoList", JSON.stringify(todoList));
    }
  };

  const removeTodo = (/** @type {number} */ index) => {
    todoList = todoList.filter((_, i) => i !== index);
    saveToLocalStorage();
  };

  const toggleComplete = (/** @type {number} */ index) => {
    todoList[index].completed = !todoList[index].completed;
    todoList = [...todoList];
    saveToLocalStorage();
  };

  const toggleHeart = (/** @type {number} */ index) => {
    todoList[index].liked = !todoList[index].liked;
    todoList = [...todoList];
    saveToLocalStorage();
  };
</script>

<header>
  <nav>
    <a href="/" class="nav__logo">JHP AI Todo</a>
    <div>
      <input
        type="text"
        bind:value={inputValue}
        placeholder="할 일을 입력하세요"
        spellcheck="false"
      />
      <button on:click={addTodo}>등록</button>
    </div>
  </nav>
</header>

<div>
  <ul>
    {#each todoList as todo, index}
      <li class:completed={todo.completed}>
        <div class="left">
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <!-- svelte-ignore a11y-no-static-element-interactions -->
          <span
            class="heart"
            on:click={() => {
              if (!todo.completed) toggleHeart(index);
            }}
          >
            {todo.liked ? "❤️" : "♡"}
          </span>
          <span class="text">{todo.text}</span>
        </div>
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <!-- svelte-ignore a11y-no-static-element-interactions -->
        <div class="remove">
          <span class="remove-span" on:click={() => removeTodo(index)}>❌</span>
          <span class="complete-span" on:click={() => toggleComplete(index)}
            >✔️</span
          >
        </div>
      </li>
    {/each}
  </ul>
</div>
