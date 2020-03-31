<script>
    import { closeModal } from "svelte-native"
    export let europa
</script>

<frame>
<page>
    <scrollView>
        <stackLayout class="container">
            <label class="h1 white" text="{europa.country}" />
            <label class="h1 white" text="Hei" />

            <button on:tap={() => closeModal()} text="Go back" />
        </stackLayout>
    </scrollView>
</page>
</frame>
<style>
    .container {
        background-color: #1b1b30;
    }
    .white {
        color: white;
        text-align: center;
    }
</style>