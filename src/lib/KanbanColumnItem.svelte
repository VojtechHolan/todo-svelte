<script lang="ts">
	import Modal from './Modal.svelte';
	import type { Validation } from 'sveltekit-superforms/index';
	import KanbanEditForm from './KanbanEditForm.svelte';
	import { taskEditSchema, type Task, type Section } from './section';
	import { superValidate } from 'sveltekit-superforms/client';
	import LinkButton from './LinkButton.svelte';

	export let form: Validation<typeof taskEditSchema>;
	export let sectionId: string;
	export let task: Task;
	export let sections: Section[];

	const editForm = superValidate({ section: sectionId, ...task }, taskEditSchema);

	type ModalData = {
		title: string;
		open: boolean;
	};
	let modal: ModalData = {
		title: 'Edit Task',
		open: false
	};

	const handleOpen = (ev: MouseEvent) => {
		ev.preventDefault();
		modal.open = true;
	};

	const handleClose = () => {
		modal.open = false;
	};
</script>

<article class="kanban-item" data-id={task.id}>
	<p class="kanban-item__text">
		{task.title}
	</p>
	<div class="kanban-item__footer">
		{#if task.field === 'high'}
			<div class="kanban-item__priority kanban-item__priority--high">
				<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512">
					<path
						fill="currentColor"
						d="M214.6 41.4c-12.5-12.5-32.8-12.5-45.3 0l-160 160c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L160 141.2V448c0 17.7 14.3 32 32 32s32-14.3 32-32V141.2L329.4 246.6c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3l-160-160z"
					/>
				</svg>
			</div>
		{:else if task.field === 'low'}
			<div class="kanban-item__priority kanban-item__priority--low">
				<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512">
					<path
						fill="currentColor"
						d="M169.4 470.6c12.5 12.5 32.8 12.5 45.3 0l160-160c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L224 370.8 224 64c0-17.7-14.3-32-32-32s-32 14.3-32 32l0 306.7L54.6 265.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3l160 160z"
					/>
				</svg>
			</div>
		{:else}
			<div class="kanban-item__priority kanban-item__priority--high">
				<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 384 512">
					<path
						fill="currentColor"
						d="M32 288c-17.7 0-32 14.3-32 32s14.3 32 32 32l384 0c17.7 0 32-14.3 32-32s-14.3-32-32-32L32 288zm0-128c-17.7 0-32 14.3-32 32s14.3 32 32 32l384 0c17.7 0 32-14.3 32-32s-14.3-32-32-32L32 160z"
					/>
				</svg>
			</div>
		{/if}

		<LinkButton href="/section/{sectionId}/tasks/{task.id}/edit" on:click={handleOpen} size="small">
			<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
				<path
					fill="currentColor"
					d="M471.6 21.7c-21.9-21.9-57.3-21.9-79.2 0L362.3 51.7l97.9 97.9 30.1-30.1c21.9-21.9 21.9-57.3 0-79.2L471.6 21.7zm-299.2 220c-6.1 6.1-10.8 13.6-13.5 21.9l-29.6 88.8c-2.9 8.6-.6 18.1 5.8 24.6s15.9 8.7 24.6 5.8l88.8-29.6c8.2-2.7 15.7-7.4 21.9-13.5L437.7 172.3 339.7 74.3 172.4 241.7zM96 64C43 64 0 107 0 160V416c0 53 43 96 96 96H352c53 0 96-43 96-96V320c0-17.7-14.3-32-32-32s-32 14.3-32 32v96c0 17.7-14.3 32-32 32H96c-17.7 0-32-14.3-32-32V160c0-17.7 14.3-32 32-32h96c17.7 0 32-14.3 32-32s-14.3-32-32-32H96z"
				/>
			</svg>
			Edit
		</LinkButton>
	</div>
</article>

{#await editForm then form}
	<Modal title={modal.title} open={modal.open} on:close={handleClose}>
		<KanbanEditForm {sectionId} {sections} {task} {form} />
	</Modal>
{/await}

<style>
	.kanban-item {
		border: 1px solid #1f3a8a0f;
		border-radius: 4px;
		margin: 1rem;
		padding: 1rem;
		background-color: rgba(255, 255, 255, 0.5);
	}

	.kanban-item__text {
		margin: 0;
		font-size: 0.9rem;
	}

	.kanban-item__footer {
		display: flex;
		align-items: center;
		justify-content: space-between;
		margin-top: 1rem;
	}

	.kanban-item__priority--low {
		color: var(--priority-low);
	}

	.kanban-item__priority--medium {
		color: var(--priority-medium);
	}

	.kanban-item__priority--high {
		color: var(--priority-high);
	}

	.kanban-item__priority svg {
		height: 1rem;
		width: auto;
	}

	.kanban-item__footer > :global(button) {
		background: transparent;
		cursor: pointer;
		display: flex;
		gap: 0.4rem;
		color: var(--primary-color);
		border: none;
		font-weight: 700;
	}

	.kanban-item__footer > :global(button) > :global(svg) {
		height: 1em;
		width: auto;
	}
</style>
