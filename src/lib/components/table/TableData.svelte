<script lang="ts">
  import Icon from '@iconify/svelte';

  import ThSort from '$lib/components/_ssd/ThSort.svelte';
  import RowCount from '$lib/components/_ssd/RowCount.svelte';
  import Pagination from '$lib/components/_ssd/Pagination.svelte';
  import { DataHandler} from '@vincjo/datatables'

  import { users } from '$lib/store/table/store'
  import { getModalStore } from '@skeletonlabs/skeleton';
  const modalStore = getModalStore();

  // Modal Examples (see also root layout)
  import ModalCreateForm from './ModalCreateForm.svelte';

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


</script>

<div class="fieldset">
    <aside>
        <button class="btn variant-filled" on:click={modalComponentCreateForm}>
            <Icon icon="wpf:add-user" class="stroke-cyan-600 text-xl"/>
            <span class="indent-1">CREATE A USER</span>
        </button>
    </aside>

    <!-- Responsive Container (recommended) -->
    <div class="table-container">
            <!-- Native Table Element -->
            <table bind:this={element} class="table table-hover">
                <thead>

                    <tr>
                        <th /> <!-- empy header -->
                        <ThSort {handler} orderBy="id">ID</ThSort>
                        <ThSort {handler} orderBy="first_name">First name</ThSort>
                        <ThSort {handler} orderBy="last_name">Last name</ThSort>
                        <ThSort {handler} orderBy="email">Email</ThSort>
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
            <footer class="flex justify-between">
        		<RowCount {handler} />
        		<Pagination {handler} />
        	</footer>
    </div>
</div>




<style>

</style>