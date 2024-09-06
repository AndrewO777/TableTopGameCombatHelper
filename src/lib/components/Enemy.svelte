<style>
    div.self {
        border-radius: 10px;
        border: 1px solid #555;
        margin: 0 auto;
        display: inline-block;
    }
    p {
        margin: .5em;
    }
    label {
        display: block;
        text-align: center;
    }
    .option {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: .5em;
        align-items: center;
        border: none;
    }
    .option input {
        height: 2em;
        width: 6em;
        margin: 0;
        padding: 0;
    }
    .option button {
        display: inline-block;
        margin: .5em;
        justify-self: end;
    }
    .option button label {
        color: #ddd;
        display: inline;
    }
    .option button:hover label {
        transition: color .6s;
        color: #fff;
    }
    .option label {
        justify-self: end;
    }
    .hide {
        display: none;
    }
</style>
<script lang="ts">

import { Attack } from '$lib';

export let name: string;
export let health: number;
export let ac: number;

//binded variables
let node:HTMLDivElement;
let attacksDiv:HTMLDivElement;
let hurtInput:number = 0;
let healthInput:number = 0;

let curHealth:number = health;
let showAddAttack:boolean = false;
let showAddStat:boolean = false;
let attacks:Attack[] = [];
let attackName:string = "";
let attackDamageBonus:number = 0;
let attackHitBonus:number = 0;
let attackDice:number[] = [0,0,0,0,0];
interface StatData {
    name:string,
    val:number
}
let stats:StatData[] = [];
let statName:string;
let statVal:number;

function Hurt(){
    if(hurtInput === undefined){
        hurtInput = 0;
    }
    curHealth -= hurtInput;
    if(curHealth <= 0){
        node.parentNode?.removeChild(node);
    }
}

function Heal(){
    if(healthInput === undefined){
        healthInput = 0;
    }
    curHealth += healthInput;
    if(curHealth > health){
        curHealth = health;
        return;
    }
}

function AddAttack(){
    attacks.push(new Attack({
        target: attacksDiv,
        props: {
            name: attackName,
            attackHitBonus: attackHitBonus,
            attackDamageBonus: attackDamageBonus,
            attackDice: attackDice
        }
    }));
    showAddAttack = !showAddAttack;
}

function AddStat(){
    let entry = {name:statName,val:statVal};
    stats = [entry, ...stats];
    showAddStat = !showAddStat;
}

</script>
<div bind:this={node} class="self">
<div class:hide={showAddAttack || showAddStat}>
    <p>Name: {name}</p>
    <p>Health: {curHealth}/{health}</p>
    <p>Armor Class: {ac}</p>
    <div>
    {#each stats as stat (stat.val)}
        <p>{stat.name}: {stat.val}</p>
    {/each}
    </div>
    <div bind:this={attacksDiv}/>
    <div class="option">
        <button on:click={Hurt}><label for="hurt">Hurt</label></button>
        <input name="hurt" type="number" bind:value={hurtInput}/>
    </div>
    <div class="option">
        <button on:click={Heal}><label for="heal">Heal</label></button>
        <input name="heal" type="number" bind:value={healthInput}/>
    </div>
    <button on:click={ ()=>showAddAttack = !showAddAttack }>Add Attack</button>
    <button on:click={ ()=>showAddStat= !showAddStat}>Add Stat</button>
    <button on:click={ ()=>node.parentNode?.removeChild(node) }>Kill</button>
</div>
<div class:hide={!showAddAttack}>
    <div class="option">
        <label for="attackName">Attack Name</label>
        <input name="attackName" type="text" bind:value={attackName}/>
    </div>
    <div class="option">
        <label for="attackHitBonus">Attack Hit Bonus</label>
        <input name="attackHitBonus" type="number" bind:value={attackHitBonus}/>
    </div>
    <div class="option">
        <label for="attackDamageBonus">Attack Damage Bonus</label>
        <input name="attackDamageBonus" type="number" bind:value={attackDamageBonus}/>
    </div>
    <label for="attackDice">Attack Dice</label>
    <div class="option">
        <label for="attackDice">d4</label>
        <input name="attackDice" type="number" bind:value={attackDice[0]}/>
        <label for="attackDice">d6</label>
        <input name="attackDice" type="number" bind:value={attackDice[1]}/>
        <label for="attackDice">d8</label>
        <input name="attackDice" type="number" bind:value={attackDice[2]}/>
        <label for="attackDice">d10</label>
        <input name="attackDice" type="number" bind:value={attackDice[3]}/>
        <label for="attackDice">d12</label>
        <input name="attackDice" type="number" bind:value={attackDice[4]}/>
    </div>
    <button on:click={AddAttack}>Add</button>
    <button on:click={ ()=>showAddAttack = !showAddAttack }>Cancel</button>
</div>
<div class:hide={!showAddStat}>
    <div class="option">
        <label for="statName">Stat Name</label>
        <input name="statName" type="text" bind:value={statName}/>
    </div>
    <div class="option">
        <label for="statVal">Stat Value</label>
        <input name="statVal" type="number" bind:value={statVal}/>
    </div>
    <button on:click={AddStat}>Add Stat</button>
    <button on:click={ ()=>showAddStat = !showAddStat}>Cancel</button>
</div>
</div>
