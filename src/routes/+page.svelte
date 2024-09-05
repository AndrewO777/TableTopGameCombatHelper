<style>
    h1 {
        text-align: center;
        font-size: 2em;
        margin: 0;
    }
    .inputBlock {
        display: grid;
        grid-template-columns: .8fr 1fr;
        grid-gap: 1em;
        justify-content: center;
        margin: 1em auto;
    }
    .inputBlock label {
        color: #999;
        text-align:right;
    }
    .inputBlock input {
        width: 20%;
        min-width: 8em;
    }
</style>
<script lang="ts">
    import { Enemy } from '$lib';

    let EnemyBin:HTMLDivElement;
    let name:string = "";
    let health:number = 0;
    let ac:number = 0;

    function AddEnemy() {
        new Enemy({
            target: EnemyBin,
            props: {
                name: name,
                health: health,
                ac: ac
            }
        });
    }
</script>
<h1>Pathfinder Combat Helper</h1>

<div bind:this={EnemyBin}>
</div>
<div class="inputBlock">
    <label for="name">Name</label>
    <input name="name" type="text" bind:value={name}/>
</div>
<div class="inputBlock">
    <label for="health">Health</label>
    <input name="health" type="number" bind:value={health}/>
</div>
<div class="inputBlock">
    <label for="ac">Armor Class</label>
    <input name="ac" type="number" bind:value={ac}/>
</div>
<button on:click={AddEnemy} class="btn">Add Enemy</button>

