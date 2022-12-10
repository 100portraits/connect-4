<style global lang='postcss'>
  
  @tailwind utilities;
  @tailwind components;
  @tailwind base;

  
</style>

<Modals>
  <div
    slot="backdrop"
    class="fixed top-0 bottom-0 left-0 right-0 bg-slate-100"
    on:click={closeModal}
  />
</Modals>

<script>

  import { Modals, closeModal } from 'svelte-modals'
  import { openModal } from 'svelte-modals'
  import Modal from './lib/Modal.svelte'

  function openRedWins() {
    openModal(Modal, { title: "Alert", message: "Red Wins!" })
  }
  function openBlueWins() {
    openModal(Modal, { title: "Alert", message: "Blue Wins!" })
  }


  let grid=[];
  let turnRed = true;
  
  resetGrid();

  function detectFour() {

    //check row
    for(let i = 0; i < grid.length; i++) {
      for(let j = 0; j < grid[i].length; j++) {
        try {
          let box=grid[i][j]
          let box2=grid[i][j+1]
          let box3=grid[i][j+2]
          let box4=grid[i][j+3]
          if (((box > 0) && (box2 > 0) && (box3 > 0) && (box4 > 0)) && ((box==box2) && (box==box3) && (box==box4))) {
            if(box==2) {
              openBlueWins()
            } else {
              openRedWins()
            }
            resetGrid();
            break;
          } else {
            continue;
          }
        }
        catch(err) {
          
        }
      }
    }

    //check column
    for(let i = 0; i < grid.length; i++) {
      for(let j = 0; j < grid[i].length; j++) {
        
      try {  
        let box=grid[i][j]
        let box2=grid[i+1][j]
        let box3=grid[i+2][j]
        let box4=grid[i+3][j]
        if (((box > 0) && (box2 > 0) && (box3 > 0) && (box4 > 0)) && ((box==box2) && (box==box3) && (box==box4))) {
          if(box==2) {
            openBlueWins()
          } else {
            openRedWins()
          }
          resetGrid();
          break;
        } else {
          continue;
        }

      } catch {

      }

      }
    }

    //check incline
    for(let i = 0; i < grid.length; i++) {
      for(let j = 0; j < grid[i].length; j++) {
        
        try {
        
        let box=grid[i][j]
        let box2=grid[i-1][j+1]
        let box3=grid[i-2][j+2]
        let box4=grid[i-3][j+3]
        if (((box > 0) && (box2 > 0) && (box3 > 0) && (box4 > 0)) && ((box==box2) && (box==box3) && (box==box4))) {
          if(box==2) {
            openBlueWins()
          } else {
            openRedWins()
          }
          resetGrid();
          break;
        } else {
          continue;
        }

      }catch {

      }
      }
    }

    //check decline
    for(let i = 0; i < grid.length; i++) {
      for(let j = 0; j < grid[i].length; j++) {
        
        try {
        
        let box=grid[i][j]
        let box2=grid[i+1][j+1]
        let box3=grid[i+2][j+2]
        let box4=grid[i+3][j+3]
        if (((box > 0) && (box2 > 0) && (box3 > 0) && (box4 > 0)) && ((box==box2) && (box==box3) && (box==box4))) {
          if(box==2) {
            openBlueWins()
          } else {
            openRedWins()
          }
          resetGrid();
          break;
        } else {
          continue;
        }

      } catch {

      }
      }
    }
  }

  function resetGrid() {
    grid=[]
    for(let i = 0; i < 6; i++) {
      grid.push([0, 0, 0, 0, 0, 0, 0])
      grid=grid
    }
  }

  function addRedToColumn(col){ 
    let firstPieceFoundAt=6;
    for(let i = 0; i < 6; i++) {
      if(grid[i][col]===1 || grid[i][col] === 2) {
        firstPieceFoundAt=i;
        break;
      }
    }
    if(firstPieceFoundAt === 0) {
      alert("column full")
    } else {
      grid[firstPieceFoundAt-1][col] = 1;
      grid=grid
    }
    turnRed=false
    detectFour();
  }

  function addBlueToColumn(col){ 
    let firstPieceFoundAt=6;
    for(let i = 0; i < 6; i++) {
      if(grid[i][col]===1 || grid[i][col] === 2) {
        firstPieceFoundAt=i;
        break;
      }
    }
    if(firstPieceFoundAt === 0) {
      alert("column full")
    } else {
      grid[firstPieceFoundAt-1][col] = 2;
      grid=grid
    }
    turnRed=true
    detectFour();
  }


</script>

<main>
  <div class='mt-12 gap-1 justify-center max-w-xl mx-auto shadow-md border-slate-800 grid grid-cols-7'>
    {#each grid as row}
      {#each row as box}
        {#if box === 0}
          <div class='bg-slate-600 p-5 text-white h-16 w-full'></div>
        {:else if box===1}
          <div class='bg-red-700 p-5 text-black h-16'></div>
        {:else}
          <div class='bg-blue-700 p-5 text-black h-16'></div>

        {/if}
      {/each}
    {/each}
  </div>

  <div class='flex justify-around gap-1 max-w-xl mx-auto mt-3'>
    {#each [0, 1, 2, 3, 4, 5, 6] as col}
    <div class='flex flex-col gap-2 w-full'>
      {#if turnRed}
        <button class='bg-red-800 text-white w-full py-2 rounded-sm shadow-md' on:click={() => addRedToColumn(col)}>+</button>
      {:else}
        <button class='bg-blue-800 text-white w-full py-2 rounded-sm shadow-md' on:click={() => addBlueToColumn(col)}>+</button>
      {/if}
    </div>
    {/each}
  </div>

</main>


