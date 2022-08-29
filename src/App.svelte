<script>
  import Table from "./components/Table.svelte";
  import { ToastContainer , FlatToast}  from "svelte-toasts";
</script>


<main class="mt-5">
  <Table></Table>
</main>
<ToastContainer placement="bottom-center" let:data={data}>
  <FlatToast {data} />
</ToastContainer>
<style>
</style>
