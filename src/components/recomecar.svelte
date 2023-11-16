<script>
  import { contatos } from "@scripts/store";

    function recomecar(){
        contatos.set([])
        window.location.href = `/`
    }
</script>
<button class="outline" on:click={()=>recomecar()}>Recomeçar</button>