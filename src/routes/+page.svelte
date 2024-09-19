<script>
    export let data;
    import Gameboy from './Gameboy.svelte';
</script>

<main>
    <Gameboy name="{data.persons.name} {data.persons.surname}"info="{data.persons.nickname}" />
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #f2f2f2;
  }
</style>
