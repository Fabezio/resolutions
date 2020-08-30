<script>
    import { useTracker } from 'meteor/rdb:svelte-meteor-data'
    import { LoginWindow, Logout } from 'meteor/levelup:svelte-accounts-ui'
    import { create_bidirectional_transition, space } from 'svelte/internal'
    import {Resolutions} from '../api/resolutions'
    import Resolution from './resolutions/Resolution.svelte'
    
    let newRes = ""
    
    function handleSubmit(evt) {
        Resolutions.insert({
            title: newRes
        })
        // /!\ don't forget to empty the resoluton string after commit
        newRes=""
    }
    // function removeRes(evt) {}
    
    $: resolutions = useTracker(() => Resolutions.find({}).fetch())
    $: user = useTracker(() => Meteor.user())
    $: userId = useTracker(() => Meteor.userId())
    // $: resolutions.length ? 
</script>

<header>
    <h1 class="text-center mt-5 mb-4">Resolutions</h1>
    <div class="row my-4">
        <div class="col-lg-6 mx-auto ">
            {#if $user}
                {#if $userId}
                     <!-- content here -->
                     <h2>{$userId}</h2>
                     <Logout />
                {:else}
                    <h3 class="alert alert-danger">user id not found</h3>
                {/if}
                 <!-- content here -->

            {:else}
                 <!-- else content here -->
                 <LoginWindow />
            {/if}
        </div>
    </div>
    <!-- add a resolution form  -->
    
    <form 
        class="mb-4" 
        on:submit|preventDefault={handleSubmit} 
        action="GET">
        <div class="w-50 mx-auto mb-4">
            <input 
                class=" mb-2 form-control" 
                type="text" 
                bind:value={newRes}
                placeholder="Add a resolution:">
            <input class=" mb-2 btn btn-block btn-secondary" type="submit" value="submit">

        </div>
    </form>

    <!-- display resolutions with alternate colors -->
    {#if $resolutions.length}
        {#each $resolutions as resolution, i}
            <Resolution {resolution} {i}>
                
            </Resolution>
        {/each}
    {:else}
        <h2 class="alert alert-danger"><span class="container">no resolution yet</span></h2>
    {/if}

</header>
