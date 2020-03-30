<script>
    import { onMount } from "svelte"

    let articlesNo = []
    let articlesLatestNo = []

    let articlesSwe = []
    let articlesLatestSwe = []

    let test 

    let countryId





    onMount( () => {
        fetch(`https://coronavirus-tracker-api.herokuapp.com/v2/locations?timelines=1`)
        .then( response => response.json() )
        .then( json => {
            articlesNo = [json.locations[205] + json.locations[175]]
            articlesLatestNo = json.locations[175].latest
            //articlesSwe = json.locations[205]
            //articlesLatestSwe = json.locations[205].latest
            console.log(articlesNo)

        }
        ).catch((err) => console.log(err))
    } )

            test = [...articlesNo, ...articlesSwe]  

            console.log(test)

</script>

<page>
    <actionBar title="Korona news" />
    <stackLayout>
        <stackLayout>
            
        </stackLayout>
    <scrollView>
        


        <flexboxLayout class="articles test">
            <flexboxLayout flexDirection="column" class="">
                <label text="Land:" />
                <label text="Smittede:" />
                <label text="Døde:" />
                <label text="Friske:" />
                <label text="Sist oppdatert:" />
            </flexboxLayout>
            <flexboxLayout flexDirection="column" class="data">
                <label text=" {articlesNo.country}" />
                <label text=" {articlesLatestNo.confirmed}" />
                <label text=" {articlesLatestNo.deaths}" />
                <label text=" {articlesLatestNo.recovered}" />
                <label text=" {articlesNo.last_updated}" />
            </flexboxLayout>
        </flexboxLayout>
    </scrollView>
    

</page>

<style>
    .articles {
        height: 100%;
        background-color: grey;
    }
    .data {
        width: 100%
    }
</style>
