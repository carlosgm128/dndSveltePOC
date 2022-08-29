<script>
  import Col from "./Col.svelte";
  export let size = 12;
  export let zone = "";
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  const onDragOver = (event) => {
  };
  const onDragEnd = (event) => {
  };
  const onDragStart = (event) => {
    console.log(event.target.parentNode.id);
    if(typeof zone !== 'undefined'){
      event.dataTransfer.setData("origin", event.target.parentNode.id);
      event.dataTransfer.setData("id", event.target.id);
    }
    event.target.classList.add("dotted");
  };
  const onDragLeave = (event) => {
  }
  const onDropHandler = (event) => {
    let id = event.dataTransfer.getData("id")
    let fromOrigin = event.dataTransfer.getData("origin");
    let data = {
      id,
      to:zone,
      from: fromOrigin
    }
    dispatch("swap", data)
  };
</script>
  <Col {size} >
    <div class="main p-2">
      <h2>{zone}</h2>
      <slot name="AddTaskForm"/>
      <div
        class="fill bg p-2"
        id={zone}
        on:dragover|preventDefault={onDragOver}
        on:dragend|preventDefault={onDragEnd}
        on:dragstart={onDragStart}
        on:drop|preventDefault={onDropHandler}
        on:dragleave={onDragLeave}
      >
        <slot/>
      </div>
    </div>
  </Col>

<style>
  .fill {
    min-height: 250px;
  }
  :global(.main) {
    border:2px solid hsla(0, 0%, 0%, 0.125);
    min-height: 250px;
    box-sizing: border-box;
    background-color: beige;
    max-width: 98%;
  }
  .bg{
    background-color: hsl(0, 0%, 100%);
    border-radius: 5px;
    border: 2px solid hsla(0, 0%, 0%, 0.125);
  }
  
  :global(.dotted) {
    border: 2px dashed hsl(0, 0%, 0%) !important;
  }
  :global(.active) {
    background-color: hsl(0, 0%, 0%);
    color: red;}
</style>
