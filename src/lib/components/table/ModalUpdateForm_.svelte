<script lang="ts">
	import type { SvelteComponent } from 'svelte';

	// Stores
	import { getModalStore } from '@skeletonlabs/skeleton';

	import { create } from '$lib/store/table/store'

	// Props
	/** Exposes parent props to this component. */
    export let parent: SvelteComponent;

	const modalStore = getModalStore();

	// Form Data
	const formData = {
        first_name: 'Jane',
		last_name: 'Doe',
        email: 'jdoe@email.com'
    };

	// We've created a custom submit function to pass the response and close the modal.
	function onFormSubmit(): void {
		if ($modalStore[0].response){
			$modalStore[0].response(formData);
			create(formData);
		}
        modalStore.close();
    }

	// Base Classes
	const cBase = 'card p-4 w-modal shadow-xl space-y-4';
	const cHeader = 'text-2xl font-bold';
	const cForm = 'border border-surface-500 p-4 space-y-4 rounded-container-token';
</script>

<!-- @component This example creates a simple form modal. -->

{#if $modalStore[0]}
	<div class="modal-example-form {cBase}">
		<header class={cHeader}>{$modalStore[0].title ?? '(title missing)'}</header>
		<article>{$modalStore[0].body ?? '(body missing)'}</article>
		<!-- Enable for debugging: -->
		<form class="modal-form {cForm}">
			<label class="label">
				<span>First Name</span>
				<input class="input" type="text" bind:value={formData.first_name} placeholder="Enter first name..." />
			</label>
			<label class="label">
				<span>Last Name</span>
				<input class="input" type="text" bind:value={formData.last_name} placeholder="Enter last name..." />
			</label>
			<label class="label">
				<span>Email</span>
				<input class="input" type="email" bind:value={formData.email} placeholder="Enter email address..." />
			</label>
		</form>
		<!-- prettier-ignore -->
		<footer class="modal-footer {parent.regionFooter}">
			<button class="btn {parent.buttonNeutral}" on:click={parent.onClose}>{parent.buttonTextCancel}</button>
			<button class="btn {parent.buttonPositive}" on:click={onFormSubmit}>Create</button>
		</footer>
	</div>
{/if}


<style>

</style>