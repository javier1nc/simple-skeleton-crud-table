<script lang="ts">
  import Icon from '@iconify/svelte';
  import { DataHandler, Datatable, Th } from '@vincjo/datatables'

  import { users } from '$lib/store/store'
  import { getModalStore } from '@skeletonlabs/skeleton';
  const modalStore = getModalStore();

  // Modal Examples (see also root layout)
  import ModalCreateForm from './ModalCreateForm.svelte';
  import ModalUpdateForm from './ModalUpdateForm.svelte';

   import { modal } from 'gros/modal'


  import Update from '$lib/components/table/Modal_Update.svelte'
  import Destroy from '$lib/components/table/Modal_Destroy.svelte'


  const handler = new DataHandler($users, { rowsPerPage: 20 })
  const rows = handler.getRows()

  let element

  $: $users, update()

  const update = () => {
	  if (element) {
		  const scrollTop = element.parentNode.scrollTop
		  handler.setRows($users)
		  setTimeout(() => {
			  element.parentNode.scrollTop = scrollTop
		  }, 2)
	  }
  }

// Custom ---

	function modalComponentCreateForm(): void {
        const c: ModalComponent = { ref: ModalCreateForm };
        const modal: ModalSettings = {
            type: 'component',
            component: c,
            title: 'Create a User',
            body: 'Complete the form below and then press Create.',
            response: (r) => console.log('response:', r)
        };
        modalStore.trigger(modal);
    }

    function modalComponentUpdateForm(): void {
        const c: ModalComponent = { ref: ModalUpdateForm };
        const modal: ModalSettings = {
            type: 'component',
            component: c,
            title: 'Update a User',
            body: 'Complete the form below and then press Update.',
            response: (r) => console.log('response:', r)
        };
        modalStore.trigger(modal);
    }

</script>

<div class="fieldset">
    <aside>
        <button class="btn variant-filled" on:click={modalComponentCreateForm}>
            <Icon icon="wpf:add-user" class="stroke-cyan-600 text-xl"/>
            <span class="indent-1">CREATE A USER</span>
        </button>
    </aside>
    <section>
        <Datatable {handler} rowsPerPage={false} search={false}>
            <table bind:this={element}>
                <thead>
                    <tr>
                        <th/>
                        <Th {handler} orderBy="id">ID</Th>
                        <Th {handler} orderBy="first_name">First name</Th>
                        <Th {handler} orderBy="last_name">Last name</Th>
                        <Th {handler} orderBy="email">Email</Th>
                    </tr>
                </thead>
                <tbody>
                    {#each $rows as row}
                        <tr>
                            <td>
                                <div class="flex">
                                    <button class="btn" on:click={() => modal.open(Update, row)}>
                                        <Icon icon="entypo:edit" class="stroke-cyan-600 text-xl"/>
                                    </button>
                                    <button class="btn" on:click={() => modal.open(Destroy, row)}>
                                        <Icon icon="ic:baseline-delete" class="stroke-cyan-600 text-xl"/>
                                    </button>
                                </div>
                            </td>
                            <td>{row.id}</td>
                            <td>{row.first_name}</td>
                            <td>{row.last_name}</td>
                            <td>{row.email}</td>
                        </tr>
                    {/each}
                </tbody>
            </table>
        </Datatable>
    </section>
</div>


<button class="btn variant-filled" on:click={modalComponentUpdateForm}>
            <Icon icon="wpf:add-user" class="stroke-cyan-600 text-xl"/>
            <span class="indent-1">Update A USER</span>
        </button>



<style>

</style>