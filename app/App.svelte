<script>
    import { onMount } from "svelte"
    import { showModal } from "svelte-native"
    import MoreInfo from "./modals/MoreInfo.svelte"
    import arraySort from 'array-sort'
    // Array med alle landene jeg har hentet fra API
    let world = []

    let infected 
    let death
    //Henter data fra API
    onMount( () => {
        fetch(`https://api.covid19api.com/summary`)
        .then( response => response.json() )
        .then( json => {
            // Europa
            world = json.Countries
            searched = world

        }
        ).catch((err) => console.log(err))
    } )
    // Search teksten
    let search = ""
    // Nytt array med filter funskjon
    let searched = []
    const filterResult = () => {
        searched = world.filter( world => world.Country.toLowerCase().includes(search.toLowerCase()))
    }
    let order = "title"
    $:  switch(order){      
            case 'title': searched = searched.sort( (a,b) => a.Country > b.Country ? 1 : -1);break
            case 'death': searched = searched.sort( (a,b) => a.TotalDeaths < b.TotalDeaths ? 1 : -1); break
            case 'infected': searched = searched.sort( (a,b) => a.TotalConfirmed < b.TotalConfirmed ? 1 : -1); break
    }
    const showNew = async (world) => {
        await showModal(
            {
                page: MoreInfo,
                props:{
                    world:world
                }
            }
        )
    }
</script>

<page style="background-color: #1b1b30;">
    <actionBar on:tap={() => console.log(searched)} title="Korona news" style="background-color: #222; color: white;"/>
    <stackLayout class="main" >
        <stackLayout class="searchbar">
            <searchBar class="search" bind:text={search} on:textChange={filterResult} hint="Search after country"/>
            <flexboxLayout class="grid">
                <button class="button" style="background-color:indigo; color:white" text="Title" on:tap={() => order = "title"} />
                <button class="button" style="background-color:indigo; color:white" text="Death" on:tap={() => order = "death"} />
                <button class="button" style="background-color:indigo; color:white" text="Infected" on:tap={() => order = "infected"} />
            </flexboxLayout>
        </stackLayout>
    <scrollView>
    <flexboxLayout flexDirection="column">
        {#each searched as item}
        {#if item.Country}
            <stackLayout class="div">
                <flexboxLayout on:tap={() => showNew(item)} flexDirection="column" class="table">
                    <label class="white h1 " text="{item.Country}" />
                    <label style="text-align:center;" class="white p" text="Total infected: {item.TotalConfirmed.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}" />
                    <label style="text-align:center;" class="white p" text="Total deaths: {item.TotalDeaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}" />
                    <label style="text-align:center;" class="white p" text="Total recovered: {item.TotalRecovered.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}" />
                    <label style="text-align:center;" class="white p" text="Click for more information" />
                </flexboxLayout>
            </stackLayout>
        {/if}
        {:else}
            <activityIndicator busy="{true}" />
        {/each}
    </flexboxLayout>
    </scrollView>
    

</page>

<style>
    .grid {
        justify-content: center;
    }
    .button {
        width: 100
    }
    .h1 {
        font-size: 25;
    }
    .main {
        background-color: #1b1b30;
    }
    .white {
        color: white;
    }
    .div {
        background-color: #1b1b30;
        padding: 10;
        height: 100%;
        justify-content: center;
        align-content: center;
    }
    .search {
        background-color: #1b1b30;
        color: white;
    }
    .table {
        width: 70%;
        height: 100%;
        padding: 5;
        margin-top: 5;
        margin-bottom: 5;
        background-color: rgba(4, 4, 4, .6);
        text-align: center;
    }
</style>