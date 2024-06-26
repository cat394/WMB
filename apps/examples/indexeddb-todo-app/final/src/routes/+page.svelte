<script lang="ts">
	import { createTodosStore, type TodosStore, type Todo } from '$lib/store/todos.svelte.js';
	import type { Category } from '$lib/store/categories.svelte.js';
	import { onMount } from 'svelte';
	import { getTodoAppDBHelper, getTodoAppDBObjectStoreHelperFactory } from '$lib/store';
	import { Todos } from '$lib/components';

	import { isEmpty } from '$lib/utils';
	import PageContainer from '$lib/components/PageContainer.svelte';

	let todosStore = $state<TodosStore | undefined>();
	let categories = $state<Category[] | undefined>();

	let creatingTodo: boolean = $state(false);

	onMount(async () => {
		const dbHelper = await getTodoAppDBHelper();
		const todosAppObjectStoreHelper = getTodoAppDBObjectStoreHelperFactory(dbHelper);

		async function makeTodosStore() {
			const todosObjectStoreHelper = todosAppObjectStoreHelper<Todo>('todos');
			const allTodos = await todosObjectStoreHelper.getAll();
			todosStore = createTodosStore(allTodos, todosObjectStoreHelper);
		}

		async function getCategories() {
			const categoriesObjectStoreHelper = todosAppObjectStoreHelper<Category>('categories');
			categories = await categoriesObjectStoreHelper.getAll();
		}

		await Promise.all([makeTodosStore(), getCategories()]);
	});

	function changeCreatingTodoState() {
		creatingTodo = !creatingTodo;
	}

	async function handleAddTodoSubmit(event: SubmitEvent) {
		if (!todosStore) return;

		changeCreatingTodoState();

		event.preventDefault();

		const form = event.target as HTMLFormElement;

		const formData = new FormData(form);

		const todo = formData.get('todo') as string;

		const categoryIds = formData.getAll('category') as string[];

		if (!todo) return;

		await todosStore.add({ todo, categoryIds });

		form.reset();

		changeCreatingTodoState();
	}
</script>

<PageContainer title="home">
	{#snippet leftContent()}
		<form onsubmit={handleAddTodoSubmit}>
			<fieldset disabled={creatingTodo}>
				<legend>Make your todo!</legend>
				<label>
					<span class="label-text">Todo:</span>
					<input type="text" name="todo" required autocomplete="off" />
				</label>
				{#if categories && !isEmpty(categories)}
					<label>
						<span class="label-text">Category:</span>
						<ul>
							{#each categories as category}
								<li>
									<label>
										<input type="checkbox" name="category" value={category.id} />
										{category.name}
									</label>
								</li>
							{/each}
						</ul>
					</label>
				{/if}
				<button class="primary-btn" type="submit">Add todo</button>
			</fieldset>
		</form>
	{/snippet}
	{#snippet rightContent()}
		<section>
			<h2>Todos</h2>
			<div class="todos-container">
				{#if todosStore}
					<div class="remaining">
						<Todos
							store={todosStore}
							todos={todosStore.store.remaining}
							header="Remaining:"
							labelMessage="Todo is completed!"
						/>
					</div>
					<div class="finished">
						<Todos
							store={todosStore}
							todos={todosStore.store.finished}
							header="Finished:"
							labelMessage="Todo is remaining!"
						/>
					</div>
				{/if}
			</div>
		</section>
	{/snippet}
</PageContainer>

<style>
	.todos-container {
		--gap: 1rem;
		--half-gap: calc(var(--gap) / 2);
		--content-base-size: 50%;
		--content-size: calc(var(--content-base-size) - var(--half-gap));

		display: grid;
		grid-template: 'remaining finished' 1fr / var(--content-size) var(--content-size);
		column-gap: var(--gap);

		.remaining {
			grid-area: remaining;
		}

		.finished {
			grid-area: finished;
		}
	}
	ul {
		display: flex;
		flex-wrap: wrap;
		gap: 1.2rem;

		label {
			width: fit-content;
		}
	}
	li {
		display: flex;
		align-items: center;
		gap: 0.8em;
	}
</style>
