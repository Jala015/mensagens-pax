<script>
  async function getContacts() {
    try {
      const contacts = await navigator.contacts.select(props, opts);
      alert(JSON.stringify(contacts));
    } catch (ex) {
      alert('o seu navegador não suporta isso')
    }
  }
</script>

<button on:click={getContacts}>Importar contatos</button>

