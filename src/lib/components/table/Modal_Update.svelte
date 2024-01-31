<script>
    import { Modal } from 'gros/modal'
    import { Input } from 'gros/form'
    import { update } from '$lib/store/table/store'
    export let close
    export let props
</script>

<Modal title="Update a user" icon="edit">
    <aside class="">
        <label class="label">
			<span class="text-black">First name</span>
			<input class="input" type="text" bind:value={props.first_name} placeholder="Enter first name..." />
		</label>
        <label class="label">
			<span class="text-black">Last name</span>
			<input class="input" type="text" bind:value={props.last_name} placeholder="Enter last name..." />
		</label>
        <label class="label">
			<span class="text-black">Email</span>
			<input class="input" type="text" bind:value={props.email} placeholder="Enter Email..." />
		</label>
    </aside>

    <svelte:fragment slot="footer">
        <button
            on:click={() => {
                update(props)
                close()
            }}>Update</button
        >
        <button on:click={close}>Cancel</button>
    </svelte:fragment>
</Modal>

<style>
    aside {
        width: 360px;
        margin: 24px;
    }
</style>