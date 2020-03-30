<script>
    import { onMount } from "svelte"


    let articlesNo = []
    let articlesLatestNo = []

    let articlesSwe = []
    let articlesLatestSwe = []

    let test = []

    let countryId

    let Europa = [1, 3, 23, 27, 30, 92, 102, 106, 107, 122, 22, 135, 130, 137, 247, 87, 146, 149, 150, 151, 155, 159, 160, 162, 166, 175, 174, 183, 184, 186, 187, 191, 194, 197, 198, 201, "storbritania", 206, 205, 91, 213, 120, 215, 129, "vatikanstaten", 16 ]

    





    onMount( () => {
        fetch(`https://coronavirus-tracker-api.herokuapp.com/v2/locations?timelines=1`)
        .then( response => response.json() )
        .then( json => {
            // Europa
            test = [json.locations[1],json.locations[3],json.locations[23],json.locations[27],json.locations[30],json.locations[92],json.locations[102],json.locations[106],json.locations[107],json.locations[122],json.locations[22],json.locations[135],json.locations[130],json.locations[137],json.locations[247],json.locations[87],json.locations[146],json.locations[149],json.locations[150],json.locations[151],json.locations[155],json.locations[159],json.locations[160],json.locations[162],json.locations[166],json.locations[175],json.locations[174],json.locations[183],json.locations[184], json.locations[186],json.locations[187],json.locations[191],json.locations[194],json.locations[197],json.locations[198],json.locations[201],json.locations[206],json.locations[205],json.locations[91],json.locations[213],json.locations[120],json.locations[215],json.locations[129],json.locations[16]]
        }
        ).catch((err) => console.log(err))
    } )

</script>

<page>
    <actionBar on:tap={() => console.log("hei")} title="Korona news Europa" style="background-color: #222; color: white;"/>
    <stackLayout class="main" >
        <stackLayout class="searchbar">
            <searchBar hint="Search after country"/>
        </stackLayout>
    <scrollView>
    <flexboxLayout flexDirection="column" class="">
    {#each test as test}
    <stackLayout class="div">
        <flexboxLayout flexDirection="column" class="table">
            <label class="white h1 " text="{test.country}" />
            <label class="white p" text="Smittede: {test.latest.confirmed}" />
            <label class="white p" text="Døde: {test.latest.confirmed}" />
            <label class="white p" text="Friske: {test.latest.recovered}" />
        </flexboxLayout>
    </stackLayout>
    {:else}
        <activityIndicator busy="{true}" />
    {/each}
    </flexboxLayout>

    </scrollView>
    

</page>

<style>
    .main {
        background-color: #777;
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
