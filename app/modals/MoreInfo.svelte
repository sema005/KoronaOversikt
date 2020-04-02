<script>
    import { closeModal } from "svelte-native"
    import { onMount } from "svelte"
    export let world


    // Gjør så det er lettere å lese tallene
    let infected = world.TotalConfirmed
    infected = infected.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
    // Gjør så det er lettere å lese tallene
    let deaths = world.TotalDeaths
    deaths = deaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
    let recovered = world.TotalRecovered
    recovered = recovered.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
    // Gjør så det er lettere å lese tallene
    let latestDeaths = world.NewDeaths
    latestDeaths = latestDeaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
    // Gjør så det er lettere å lese tallene
    let latestConfirmed = world.NewConfirmed
    latestConfirmed = latestConfirmed.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")
    // Gjør så det er lettere å lese tallene
    let latestRecovered= world.NewRecovered
    latestRecovered = latestRecovered.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")

</script>

<frame>
<page>
<actionbar  title="{world.Country}" class="actionbar"/>
    <scrollView>
        <flexboxLayout class="container" flexDirection="column">
                <flexboxLayout class="box center" flexDirection="column" row="0" col="0">
                    <stackLayout class="image_container">
                        <image src="~/image/virus4.png" class="image" />
                    </stackLayout>
                    <label class="h1 orange" text="{infected}" />
                    <label class="p orange" text="Total confirmed cases" />
                </flexboxLayout>
                <flexboxLayout class="box center" flexDirection="column" row="0" col="1">
                    <stackLayout class="image_container">
                        <image src="~/image/danger.png" class="image" />
                    </stackLayout>
                    <label class="h1 red" text="{deaths}" />
                    <label class="p red" text="Total deaths" />
                </flexboxLayout>
                <flexboxLayout class="box center" flexDirection="column" row="0" col="2">
                    <stackLayout class="image_container">
                        <image src="~/image/thumbsUp3.png" class="image" />
                    </stackLayout>
                    <label class="h1 green" text="{recovered}" />
                    <label class="p green" text="Confirmed revoceries" />
                </flexboxLayout>
                <flexboxLayout class="box center" flexDirection="column" row="0" col="0">
                    <stackLayout class="image_container">
                        <image src="~/image/virus4.png" class="image" />
                    </stackLayout>
                    <label class="h1 orange" text="{latestConfirmed}" />
                    <label class="p orange" text="Latest confirmed" />
                </flexboxLayout>
                <flexboxLayout class="box center" flexDirection="column" row="0" col="1">
                    <stackLayout class="image_container">
                        <image src="~/image/danger.png" class="image" />
                    </stackLayout>
                    <label class="h1 red" text="{latestDeaths}" />
                    <label class="p red" text="Latest deaths" />
                </flexboxLayout>
                <flexboxLayout class="box center" flexDirection="column" row="0" col="2">
                    <stackLayout class="image_container">
                        <image src="~/image/thumbsUp3.png" class="image" />
                    </stackLayout>
                    <label class="h1 green" text="{latestRecovered}" />
                    <label class="p green" text="Latest recovered" />
                </flexboxLayout>
                <button  on:tap={() => closeModal()} text="Go back"  class="button" />
        </flexboxLayout>
    </scrollView>
</page>
</frame>
<style>
    .actionbar {
        background-color: #261c49;
        color: white;
    }

    .container {
        background-color: #261c49;
        padding: 10;
    }
    .box {
        background-color: #10163a;
        margin-bottom: 9;
        margin-top: 8;
        border-radius: 20;
        height: 200;
    }
    .center {
        text-align: center;
        justify-content: center;
    }
    .p {
        font-size: 20;
    }
    .orange {
        color: orange;
    }
    .red {
        color: red;
    }
    .green {
        color: darkgreen;
    }
    .button {
        background-color: indigo;
        color: white;
        border-radius: 20;
    }
    .image_container {
        height: 50;
    }
</style>
