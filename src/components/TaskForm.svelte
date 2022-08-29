<script>
import { createEventDispatcher } from "svelte";
import {v4 as uuidv4 } from 'uuid'
  let values = {
    id:undefined,
    name: "",
  };

  const dispatch = createEventDispatcher();
  const onSubmitHandle = () => {
    dispatch('addTask', {...values, id:uuidv4()})
    values = {
      id:undefined,
      name: "",
    }
  }
</script>
<div class="p-2" style="width: 98%;margin:0 auto;">
  <form class="form" on:submit|preventDefault={onSubmitHandle}>
    <div class="row">
      <div class="col-9">
        <input class="form-control" type="text" bind:value={values.name} />
      </div>
      <div class="col-3">
        <button type="submit" class="btn btn-danger"> add </button>
        <button type="reset" class="btn btn-secondary"> clear </button>
      </div>
    </div>
  </form>
</div>
