<script>
    import { onMount } from "svelte"

    let articles = []
    let articlesLatest = []
    let id = [ 25, 175]



    onMount( () => {
        fetch(`https://coronavirus-tracker-api.herokuapp.com/v2/locations?timelines=1`)
        .then( response => response.json() )
        .then( json => {
            console.log("Resultater")
            articles = json.locations[id]
            articlesLatest = json.locations[id].latest
            console.log(articles)
        }
        ).catch((err) => console.log(err))
    } )

</script>

<page>
    <actionBar title="Korona news" />
    <stackLayout class="articles">
        {#each articles as article}
            <stackLayout>
                <label text={article.country} />
                <label text="Hei" />
            </stackLayout>
        {:else}
            <activityIndicator busy="{true}" />
        {/each}
    </stackLayout>



</page>

<style>

</style>
