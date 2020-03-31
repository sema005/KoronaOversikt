<script>
    import { closeModal } from "svelte-native"
    import { onMount } from "svelte"
    export let europa

    // Gjør så det er lettere å lese tallene
    let population = europa.country_population.toLocaleString()
    population = population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")
    // Gjør så det er lettere å lese tallene
    let infected = europa.latest.confirmed
    infected = infected.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")
    // Gjør så det er lettere å lese tallene
    let deaths = europa.latest.deaths
    deaths = deaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")

    // Splitter datoen og tiden så det er lettere å lese
    let dateAndTime = europa.last_updated
    let last_updatedDate = dateAndTime.substring(0, 10);
    let last_updatedClock = dateAndTime.substring(11, 19);
</script>

<frame>
<page>
<actionbar  title="More information" class="actionbar"/>
    <scrollView>
        <stackLayout class="container">
            <label class="h1 center white" text="{europa.country}" />
            <label class="p white" text="Population: {population} " />
            <label class="p white" text="Confirmed infected: {infected}" />
            <label class="p white" text="Confirmed deaths: {deaths} " />
            <!--Api´et har ikke noe data på hvor mange som er blitt friske-->
            <label on:tap={() => console.log(last_updatedClock)} class="p white" text="Last updated: {last_updatedDate + ", " + last_updatedClock}" />
            <!--Få til en chart hvis det går ann-->
            <button  on:tap={() => closeModal()} text="Go back"  class="button" />
        </stackLayout>
    </scrollView>
</page>
</frame>
<style>
    .actionbar {
        background-color: #222;
        color: white;
    }

    .container {
        background-color: #1b1b30;
        justify-content: center;
        align-content: center;
        padding: 10
    }
    .center {
        text-align: center;
    }
    .white {
        color: white;
    }
    .button {
        background-color: indigo;
        color: white;
        position: absolute;
        border-radius: 20;
        position: absolute;
        bottom: 0;
    }

    .p {
        padding-left: 10;
    }
</style>