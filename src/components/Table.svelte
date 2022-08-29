<script>
  import Col from "./Col.svelte";
  import Container from "./Container.svelte";
  import DragItem from "./DragItem.svelte";
  import DragSpace from "./DragSpace.svelte";
  import Row from "./Row.svelte";
  import { v4 as uuidv4 } from "uuid";
  import TaskForm from "./TaskForm.svelte";
  import {toasts} from "svelte-toasts";

  let tasksQueue = [
    { id: uuidv4(), name: "task1" },
    { id: uuidv4(), name: "task2" },
    { id: uuidv4(), name: "task3" },
  ];
  let tasksProgress = [{ id: uuidv4(), name: "task4" }];
  let tasksDone = [];
  const onSwap = (event) => {
    swap(event.detail);
  };
  const swap = (payload) => {
    let value;
    switch (payload.from) {
      case "queue":
        console.log("movig start");
        value = tasksQueue.find((x) => x.id === payload.id);
        console.log("encontrado ->", value);
        console.log("removing ", value, "from ", payload.from);
        tasksQueue = tasksQueue.filter((x) => x.id !== payload.id);
        console.log("movig ", value, " from ", payload.from, " finish");
        break;
      case "progress":
        console.log("movig start");
        value = tasksProgress.find((x) => x.id === payload.id);
        console.log("encontrado ->", value);
        console.log("removing ", value, "from ", payload.from);
        tasksProgress = tasksProgress.filter((x) => x.id !== payload.id);
        console.log("movig ", value, " from ", payload.from, " finish");
        break;
      case "done":
        console.log("movig start");
        value = tasksDone.find((x) => x.id === payload.id);
        console.log("encontrado ->", value);
        console.log("removing ", value, "from ", payload.from);
        tasksDone = tasksDone.filter((x) => x.id !== payload.id);
        console.log("movig ", value, " from ", payload.from, " finish");
        break;
      default:
        break;
    }
    switch (payload.to) {
      case "queue":
        console.log("adding start");
        console.log("encontrado ->", value);
        console.log("before ->", tasksQueue);
        tasksQueue = [...tasksQueue, value];
        console.log("after ->", tasksQueue);
        break;
      case "progress":
        console.log("adding start");
        console.log("encontrado ->", value);
        console.log("before ->", tasksProgress);
        tasksProgress = [...tasksProgress, value];
        console.log("after ->", tasksProgress);
        break;
      case "done":
        console.log("adding start");
        console.log("encontrado ->", value);
        console.log("before ->", tasksDone);
        tasksDone = [...tasksDone, value];
        console.log("after ->", tasksDone);
        break;
      default:
        break;
    }
  };
  const onAddTask = (event) => {
    console.log(event);
    tasksQueue = [...tasksQueue, event.detail]
    toasts.success('Success', 'task added',{ duration: 4000,showProgress:true })
  }
</script>

<Container>
  <Row>
    <TaskForm on:addTask={onAddTask} />
  </Row>
  <Row>
    <DragSpace zone={"queue"} size={4} on:swap={onSwap}>
      {#each tasksQueue as item (item.id)}
        <DragItem {...item} />
      {/each}
    </DragSpace>

    <DragSpace zone={"progress"} size={4} on:swap={onSwap}>
      {#each tasksProgress as item (item.id)}
        <DragItem {...item} />
      {/each}
    </DragSpace>

    <DragSpace zone={"done"} size={4} on:swap={onSwap}>
      {#each tasksDone as item (item.id)}
        <DragItem {...item} class="done" />
      {/each}
    </DragSpace>
  </Row>
</Container>

<style>
  :global(.done) {
    text-decoration: line-through;
  }
  :global(.custom-gap){
    column-gap: 2rem
  }
</style>
