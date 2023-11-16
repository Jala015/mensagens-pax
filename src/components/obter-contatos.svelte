<script>
  async function getContacts() {
    try {
        const props = await navigator.contacts.getProperties();
      const contacts = await navigator.contacts.select(props, {multiple:true});
      alert(JSON.stringify(contacts));
    } catch (ex) {
      alert(ex)
    }
  }
</script>

<button on:click={getContacts}>Importar contatos</button>

