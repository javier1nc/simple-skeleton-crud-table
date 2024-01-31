<script>
    import { Modal } from 'gros/modal'
    import { destroy } from '$lib/store/table/store'
    export let close
    export let props
</script>

<Modal title="Remove a user" icon="delete_forever" class="alert variant-filled-warning">
    <aside class="m-12 w-96 text-[#e57373]">
        Are you sure you want to permanently delete the user "<strong>{props.first_name} {props.last_name} </strong>"?

    </aside>

    <svelte:fragment slot="footer">
        <button
            on:click={() => {
                destroy(props)
                close()
            }}>Delete</button
        >
        <button on:click={close}>Cancel</button>
    </svelte:fragment>
</Modal>

<style>
    aside {
    }
</style>