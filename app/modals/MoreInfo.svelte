<script>
    import { closeModal } from "svelte-native"
    export let europa

    // Legger til mellomrom mellom tallene så det blir lettere å lese
    // Antall mennesker som lever i landet
    let population = europa.country_population.toLocaleString()
    population = population.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")

    let infected = europa.latest.confirmed
    infected = infected.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")

    let deaths = europa.latest.deaths
    deaths = deaths.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")

</script>

<frame>
<page>
    <scrollView>
        <stackLayout class="container">
            <label class="h1 center white" text="{europa.country}" />
            <label class="p white" text="{population} lives in {europa.country}" />
            <label class="p white" text="It is {infected} confirmed infected in {europa.country}" />
            <label class="p white" text="It is {deaths} peoples that has died by COVID-19" />
            <!--Api har ikke noe data på hvor mange som er blitt friske-->

            



            <button on:tap={() => closeModal()} text="Go back" />
        </stackLayout>
    </scrollView>
</page>
</frame>
<style>
    .container {
        background-color: #1b1b30;
    }
    .center {
        text-align: center;
    }
    .white {
        color: white;
    }

    .p {
        padding-left: 10;
    }
</style>