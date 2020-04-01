<script>
    import { onMount } from "svelte"
    import { showModal } from "svelte-native"
    import MoreInfo from "./modals/MoreInfo.svelte"
    import arraySort from 'array-sort'
    // Array med alle landene jeg har hentet fra API
    let europa = []
    let infected 
    let death
    // ID til alle landene fra API som er i Europa
    let EuropaID = [1, 3, 23, 27, 30, 92, 102, 106, 107, 122, 22, 135, 130, 137, 247, 87, 146, 149, 150, 151, 155, 159, 160, 162, 166, 175, 174, 183, 184, 186, 187, 191, 194, 197, 198, 201, "storbritania", 206, 205, 91, 213, 120, 215, 129, "vatikanstaten", 16 ]
    /*
    onMount( () => {
        fetch(`https://coronavirus-tracker-api.herokuapp.com/v2/locations`)
        .then( response => response.json() )
        .then( json => {
            console.log(json.locations[0])
            // Europa
            europa = [json.locations[1],json.locations[3],json.locations[23],json.locations[27],json.locations[30],json.locations[92],json.locations[102],json.locations[106],json.locations[107],json.locations[122],json.locations[22],json.locations[135],json.locations[130],json.locations[137],json.locations[247],json.locations[87],json.locations[146],json.locations[149],json.locations[150],json.locations[151],json.locations[155],json.locations[159],json.locations[160],json.locations[162],json.locations[166],json.locations[175],json.locations[174],json.locations[183],json.locations[184], json.locations[186],json.locations[187],json.locations[191],json.locations[194],json.locations[197],json.locations[198],json.locations[201],json.locations[206],json.locations[205],json.locations[91],json.locations[213],json.locations[120],json.locations[215],json.locations[129],json.locations[16]]
            searched = europa
        }
        ).catch((err) => console.log(err))
    } )
    */
    onMount( () => {
        fetch(`https://api.covid19api.com/summary`)
        .then( response => response.json() )
        .then( json => {
            // Europa
            europa = json.Countries
            searched = europa
            console.log(europa)
        }
        ).catch((err) => console.log(err))
    } )
    // Search teksten
    let search = ""
    // Nytt array med filter funskjon
    let searched = []
    const filterResult = () => {
        searched = europa.filter( europa => europa.Country.toLowerCase().includes(search.toLowerCase()))
    }
    let order = "title"
    $:  switch(order){      
            case 'title': searched = searched.sort( (a,b) => a.Country > b.Country ? 1 : -1);break
            case 'death': searched = searched.sort( (a,b) => a.TotalDeaths < b.TotalDeaths ? 1 : -1); break
            case 'infected': searched = searched.sort( (a,b) => a.TotalConfirmed < b.TotalConfirmed ? 1 : -1); break
    }
    const showNew = async (europa) => {
        await showModal(
            {
                page: MoreInfo,
                props:{
                    europa:europa
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
                    <label style="text-align:center;" class="white p" text="Total infected: {item.TotalConfirmed.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}" />
                    <label style="text-align:center;" class="white p" text="Total deaths: {item.TotalDeaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}" />
                    <label style="text-align:center;" class="white p" text="Total recovered: {item.TotalRecovered.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")}" />
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