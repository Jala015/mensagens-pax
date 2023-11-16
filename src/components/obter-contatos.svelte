<script>
  import { contatos } from "@scripts/store";

    let contatosLocal = []
    let pronto = false

    function formatarResultados(r){
        r.forEach((contato)=>{
            contatosLocal = [...contatosLocal, {nome: contato.name ?? '', contato: contato.tel[0] ?? undefined}]
        })
        contatos.set(contatosLocal)
        pronto = true
    }

  async function getContacts() {
    try {
        const props = await navigator.contacts.getProperties();
      const contacts = await navigator.contacts.select(props, {multiple:true});
      formatarResultados(contacts)
    } catch (ex) {
      alert(ex)
    }
  }


</script>

<button on:click={getContacts}>Importar contatos</button>

{#if pronto}
<a href="/lista-de-contatos" role="button">Próximo</a>
{/if}