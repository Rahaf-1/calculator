<svelte:head>
    <title>
        الة حاسبة
    </title>
</svelte:head>
<script  lang="ts">
	import Divison from './../lib/icons/divison.svelte';
    import Minus from "$lib/icons/minus.svelte";
    import Multiplication from "$lib/icons/multilication.svelte";
    import Addition from "$lib/icons/addition.svelte";
    import Delete from "$lib/icons/delete.svelte";
    import Equal from "$lib/icons/equal.svelte";
    import {onMount} from "svelte";

    function backSpace() {
        switch(equation.substring(equation.length - 3 ,equation.length)) {
            case " * ":
            case " + ":
            case " / ":
            case " - ":
                equation = equation.substring(0,equation.length-3);
                break;
            default:
            equation = equation.substring(0,equation.length-1);
        }
    }
    function addToEquation(value: string) {
      equation +=value;
    }
    function clear(){
        equation="";
    }
    function solve(){
        try{
            let answer = eval(equation);
            if (answer == undefined) throw SyntaxError;
            equation = answer;
        } catch (error) {
            let output = document.getElementById("output");
            output?.classList.add("bg-red-500");
            setTimeout(() => {
              output?.classList.remove("bg-red-500");
            }, 500);
        }
    }
    let equation: string="";
    function onKeyDown(e: KeyboardEvent){
        new Audio('/click.wav').play();
        let button = document.getElementById(e.key);
        button?.click();
        button?.focus();
        setTimeout(() => {
            //@ts-ignore
            document.activeElement?.blur();
        }, 100);
    }

    onMount(() => {
     let allButtons = document.getElementsByTagName('button');
     for (let i=0; i<allButtons.length; i++){
        allButtons[i].addEventListener('click', () =>{
          new Audio('/click.wav').play();
        });
     }
    });

    </script>

    <svelte:window on:keydown|preventDefault={onKeyDown} />

<div class= "bg-white rounded-3xl grid grid-cols-4 gap-1 p-6 text-zinc-800 font-semibold text-xl shadow-xl max-w-[16rem]">
<div
 id="output"
 class="bg-slate-200 rounded-md col-span-4 min-h-16 flex items-center px-4 mb-3 break-all transition-all">
   {equation}
</div>
<button id="Delete" on:click={clear} class="bg-red-100 active:bg-red-200">C</button>
<button id="Backspace" on:click={backSpace} class="bg-red-100 active:bg-red-200">
    <Delete />
</button>
<button id="%" on:click={() => addToEquation(" /100")} class="bg-red-100 active:bg-red-200 ">%</button>
<button id="-" on:click={() => addToEquation(' - ')} class="bg-red-300 text-white active:bg-red-400">
    <Minus /> 
 </button>
<button id="1" on:click={() => addToEquation('1')} class="active:bg-red-300">1</button>
<button id="2" on:click={() => addToEquation('2')} class="active:bg-red-300">2</button>
<button id="3" on:click={() => addToEquation('3')} class="active:bg-red-300">3</button>
<button id="/" on:click={() => addToEquation(' / ')}  class="bg-red-400 text-white active:bg-red-500">
    <Divison />
   </button>
<button id="4" on:click={() => addToEquation('4')} class="active:bg-red-300">4</button>
<button id="5" on:click={() => addToEquation('5')} class="active:bg-red-300">5</button>
<button id="6" on:click={() => addToEquation('6')} class="active:bg-red-300">6</button>
<button id="*" on:click={() => addToEquation(' * ')} class="bg-red-500 text-white active:bg-red-600">
    <Multiplication />
</button>
<button id="7" on:click={() => addToEquation('7')} class="active:bg-red-300">7</button>
<button id="8" on:click={() => addToEquation('8')} class="active:bg-red-300">8</button>
<button id="9" on:click={() => addToEquation('9')} class="active:bg-red-300">9</button>
<button id="+" on:click={() => addToEquation(' + ')} class="bg-red-600 text-white active:bg-red-700">
    <Addition />
</button>
<button id="0" on:click={() => addToEquation('0')} class="active:bg-red-300">0</button>
<button id="." on:click={() => addToEquation('.')} class="active:bg-red-300">.</button>
<button id="=" on:click={() =>solve()} class="bg-red-100 col-span-2 active:bg-red-200">
    <Equal />
</button>
</div>
