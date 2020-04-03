<script>
    import { onMount } from "svelte"
    import { showModal } from "svelte-native"
    import MoreInfo from "./modals/MoreInfo.svelte"
    import arraySort from 'array-sort'
    // Array med alle landene jeg har hentet fra API
    let world = []

    //Henter data fra API
    onMount( () => {
        fetch(`https://api.covid19api.com/summary`)
        .then( response => response.json() )
        .then( json => {
            // World
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

    // Filtrer etter hva du trykker på
    let order = "title"
    $:  switch(order){      
            case 'title': searched = searched.sort( (a,b) => a.Country > b.Country ? 1 : -1);break
            case 'death': searched = searched.sort( (a,b) => a.TotalDeaths < b.TotalDeaths ? 1 : -1); break
            case 'infected': searched = searched.sort( (a,b) => a.TotalConfirmed < b.TotalConfirmed ? 1 : -1); break
            case 'recovered': searched = searched.sort( (a,b) => a.TotalRecovered < b.TotalRecovered ? 1 : -1); break
    }

    // Åpner en ny modal som viser mer informasjon
    const showNew = async (world) => {
        await showModal(
            {
                page: MoreInfo,
                fullscreen: true,
                props:{
                    world:world
                }
            }
        )
    }
</script>

<page style="background-color: #1b1b30;">
    <actionBar title="Korona NEWS" style="background-color: #261c49;color: white;"/>
    <stackLayout class="main" >
        <stackLayout class="container_search_buttons">
            <actionItem ios.systemIcon="" ios.position="right" text="Test"/>
                <searchBar class="search" bind:text={search} on:textChange={filterResult} hint="Search after country"/>
            <flexboxLayout class="container_buttons">
                <button class="button" text="Title" on:tap={() => order = "title"} />
                <button class="button" text="Death" on:tap={() => order = "death"} />
                <button class="button" text="Infected" on:tap={() => order = "infected"} />
                <button class="button" text="Recovered" on:tap={() => order = "recovered"} />
            </flexboxLayout>
        </stackLayout>
    <scrollView>
    <flexboxLayout flexDirection="column">
        {#each searched as item}
        {#if item.Country}
            <stackLayout class="div">
                <flexboxLayout on:tap={() => showNew(item)} flexDirection="column" class="table">
                    <label class="white h1 " text="{item.Country}" />
                    <label style="text-align:center;" class="orange p" text="Total infected: {item.TotalConfirmed.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}" />
                    <label style="text-align:center;" class="red p" text="Total deaths: {item.TotalDeaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}" />
                    <label style="text-align:center;" class="green p" text="Total recovered: {item.TotalRecovered.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}" />
                    <label style="text-align:center;" class="white p" text="Tap for more information" />
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
    .container_buttons {
        justify-content: center;
        width: 100%;
    }
    .button {
        width: 200;
        background-color:indigo;
        color: white;
        font-size: 12;
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
    .red {
        color: red;
    }
    .orange {
        color: orange;
    }
    .green {
        color: green;
    }
    .div {
        background-color: #1b1b30;
        padding: 10;
        height: 100%;
        justify-content: center;
        align-content: center;
    }
    .search {
        color: white;
        background-color: #1b1b30;
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