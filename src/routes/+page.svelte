<script lang="ts">
  import { onMount } from 'svelte';
  import { fade, scale } from 'svelte/transition';
  import { flip } from 'svelte/animate';

  import { Input } from '$lib/components/ui/input';
  import { Button } from '$lib/components/ui/button';
  import { Separator } from '$lib/components/ui/separator';

  import { Trash, Check, Plus, X } from 'lucide-svelte';

  interface Todo {
    id: number;
    text: string;
    completed: boolean;
  }

  interface ListData {
    title: string;
    todos: Todo[];
  }

  let listData: ListData = {
    title: '',
    todos: []
  };

  let todoText: string = '';

  // Load data from local storage on mount
  function loadDataFromLocalStorage(): void {
    try {
      const existingData = localStorage.getItem('listData');
      listData = existingData
        ? JSON.parse(existingData)
        : { title: '', todos: [] };
    } catch (error) {
      console.error('Error loading data from localStorage:', error);
      listData = { title: '', todos: [] };
    }
  }

  onMount(loadDataFromLocalStorage);

  // Update local storage with list data
  function updateLocalStorage(): void {
    localStorage.setItem('listData', JSON.stringify(listData));
  }

  // Add a new todo
  function addTodo(): void {
    if (todoText.length <= 0) return;
    const newTodo: Todo = { id: Date.now(), text: todoText, completed: false };
    listData.todos = [newTodo, ...listData.todos];
    updateLocalStorage();
    todoText = ''; // clear field
  }

  // Update the text of a todo
  function updateTodo(id: number, newText: string): void {
    listData.todos = listData.todos.map((todo) =>
      todo.id === id ? { ...todo, text: newText } : todo
    );
    updateLocalStorage();
  }

  // Toggle the completed status of a todo
  function toggleTodoStatus(id: number, newStatus: boolean): void {
    listData.todos = listData.todos.map((todo) =>
      todo.id === id ? { ...todo, completed: newStatus } : todo
    );

    if (newStatus) {
      const completedTodo = listData.todos.find((todo) => todo.id === id);
      listData.todos = listData.todos.filter((todo) => todo.id !== id);
      if (completedTodo) {
        listData.todos.push(completedTodo);
      }
    }

    updateLocalStorage();
  }

  // Delete a todo
  function deleteTodo(id: number): void {
    listData.todos = listData.todos.filter((todo) => todo.id !== id);
    updateLocalStorage();
  }

  // Clear the entire list
  function clearList(): void {
    listData.title = '';
    listData.todos = [];
    updateLocalStorage();
  }
</script>

<svelte:head>
  <title>ToDo by Julian Rocha</title>
  <meta
    name="description"
    content="A user-friendly web application designed to help you stay organized and manage your tasks effectively."
  />
</svelte:head>

<main class="mx-auto my-16 w-full max-w-sm px-4 sm:px-0 md:max-w-md">
  <!-- List Configuration -->
  <div class="mb-2 flex items-center justify-between">
    <!-- Title Input -->
    <Input
      bind:value={listData.title}
      on:input={updateLocalStorage}
      type="text"
      placeholder="Add Title"
      class="max-w-fit border-0 bg-transparent text-2xl font-bold"
      aria-label="List Title"
    />
    <!-- Clear List Button -->
    <Button variant="ghost" on:click={clearList} aria-label="Clear List">
      <Trash />
    </Button>
  </div>

  <!-- Add Todo Form -->
  <form on:submit|preventDefault class="flex gap-2">
    <Input
      bind:value={todoText}
      type="text"
      placeholder="e.g., Buy groceries"
      aria-label="New Todo Input"
    />
    <Button
      on:click={addTodo}
      type="submit"
      disabled={todoText.length === 0}
      aria-label="Add Todo Button"
    >
      <Plus />
    </Button>
  </form>

  <Separator class="my-10" />

  <!-- List -->
  <ul class="flex flex-col gap-4">
    {#each listData.todos as todo (todo.id)}
      <div in:fade out:scale|local animate:flip={{ duration: 350 }}>
        <li class="flex gap-2">
          <Input
            bind:value={todo.text}
            on:input={() => updateTodo(todo.id, todo.text)}
            type="text"
            class={todo.completed ? 'line-through' : ''}
            disabled={todo.completed}
            aria-label={`Todo Text for ${todo.text}`}
          />
          {#if !todo.completed}
            <Button
              on:click={() => toggleTodoStatus(todo.id, !todo.completed)}
              variant="secondary"
              aria-label="Complete Todo Button"
            >
              <Check />
            </Button>
          {/if}
          <Button
            variant="destructive"
            on:click={() => deleteTodo(todo.id)}
            aria-label="Delete Todo Button"
            ><X />
          </Button>
        </li>
      </div>
    {/each}
  </ul>
</main>
