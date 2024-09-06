<style>
    div {
        border-radius: 10px;
        border: 1px solid #555;
        margin: auto;
    }
    p {
        margin: .5em;
    }
    button {
        display: inline-block;
        margin: .5em;
    }
    .roll {
        display: inline-block;
        padding: 0;
        margin: 0;
        margin-left: 4px;
    }
    .attackRoll {
        border: none;
    }
</style>
<script lang="ts">

export let name:string;
export let attackHitBonus:number;
export let attackDamageBonus:number;
export let attackDice:number[];
//attackDice index 0 is the d4 count index 1 is the d6 count, etc.

//binded variables
let node:HTMLDivElement;

interface DamageData {
    value:number,
    id:string
}

let damageRolls:DamageData[] = [];
let totalDamage:number = 0;
let attackRoll:number = 0;

function RollAttack(){
    attackRoll = Math.floor(Math.random()*20+1);
    let dieSize:number = 4;
    damageRolls = [];
    totalDamage = 0;
    for(let i:number = 0; i < attackDice.length; ++i){
        for(let j:number = 0; j < attackDice[i]; ++j){
            let entry = {value: Math.floor(Math.random()*dieSize+1), id: dieSize.toString() + j.toString()} ;
            damageRolls = [entry,...damageRolls];
            totalDamage += entry.value;
        }
        dieSize += 2;
    }
    totalDamage += attackDamageBonus;
}

</script>
<div bind:this={node}>
    <p>Name: {name}</p>
    <div class="attackRoll">
    {#if attackRoll > 0}
    <p>{attackRoll} + {attackHitBonus} = {attackRoll + attackHitBonus}</p>
    {/if}
    {#each damageRolls as roll (roll.id)}
        <p class="roll">{roll.value},</p>
    {/each}
    {#if damageRolls.length > 0}
    <p class="roll">+ {attackDamageBonus} = {totalDamage}</p>
    {/if}
    </div>
    <button on:click={RollAttack}>Roll Attack</button>
    <button on:click={ ()=>node.parentNode?.removeChild(node) }>Remove</button>
</div>
