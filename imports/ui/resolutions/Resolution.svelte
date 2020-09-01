<script>
    // import {createEventDispatcher} from 'svelte'
    import {Resolutions} from '../../api/resolutions'
    export let resolution
    export let i
    let updateRes = resolution.title

    let display = "btn"

    // const dispatch = createEventDispatcher()

    function deleteRes() {
        Resolutions.remove(resolution._id)
        // dispatch('delete', i)
    }
    function handleUpdate(evt) {
        changeDisplay()
        Resolutions.update({
            _id: resolution._id
        },{
                
            title: updateRes
        })
    }
    function changeDisplay () {
        display === "btn" ? display = "input" : display = "btn"
    }

</script>
<div class="mx-auto w-50 alert alert-{i%2 == 0 ? 'success' : 'warning'} ">
    <h2 class="container">{resolution.title}</h2>
    <div class="float-right">
        {#if display === "btn"}
            <button on:click={changeDisplay} class="badge border-0 badge-pill badge-warning">&#9889;</button>
            |
            <span class="badge badge-pill badge-danger" on:click={deleteRes} >&times;</span>
        {:else}
            <form on:submit|preventDefault={handleUpdate}>
                <input type="text" bind:value={updateRes} placeholder={updateRes} >
                <h6 class="badge badge-pill badge-primary" on:click={handleUpdate} type="submit">Update</h6>
                <h6 class="badge badge-pill badge-secondary" on:click={changeDisplay} >cancel</h6>
            </form>
        {/if}

    </div>
    
    <slot />
</div>

<style>
    h2 {
        display: inline;
    }
    .delete {
        cursor: pointer;
    }

    .delete:hover {
        font-weight: bold;
    }
    
</style>