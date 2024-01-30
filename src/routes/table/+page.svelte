<script lang="ts">
    import TableData from '$lib/components/table/TableData.svelte'
    import data from '$lib/data/data'
    import { users } from '$lib/store/store'

    $users = data

</script>

<section>
<TableData users={$users}/>
</section>

<style></style>