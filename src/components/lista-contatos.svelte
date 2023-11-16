<script>
  import { contatos } from "@scripts/store";

  function toggleItem(contato) {
    contatos.update((contatosList) => {
      const index = contatosList.findIndex((c) => c.contato === contato);

      if (index !== -1) {
        contatosList[index].status = !contatosList[index].status;
      }
      return contatosList;
    });
  }

  function formatPhoneNumber(number) {
    const phoneNumberString = String(number);
    return phoneNumberString.replace(/\D/g, ""); // Remove caracteres não numéricos
  }

  function getWhatsAppLink(item) {
    try {
      const formattedNumber = formatPhoneNumber(item.contato);
      const encodedMessage = encodeURI(
        value.replace(/\{nome\}/g, item.nome ?? "NOME").replace("%0A","\n")
      ).replace("%0A","%0a");
      console.log(encodedMessage)
      return `https://wa.me/${formattedNumber}?text=${encodedMessage}`;
    } catch (error) {
      console.error(error);
      return "";
    }
  }

  let value = `Oi, {nome}! Bom dia! Tudo bem? Espero que sim! 

Já peço desculpas pela mensagem longa, mas é por uma boa causa 🤍

Como faço todos os anos, estou participando da Gincana de Natal do Movimento Pax, do Colégio Santo Américo, com o objetivo principal de arrecadação de cestas básicas para famílias das comunidades de Paraisópolis e Jardim Colombo! 

Fazemos as entregas de cestas mensalmente às centenas de famílias cadastradas, que contam todo ano com a nossa ajuda 🙏

Ano passado, conseguimos 9.821 cestas e ajudamos MUITAS famílias ao longo de 2023! 
O que acha de contribuir para batermos essa meta? 

Cada cesta custa R$75,00, mas QUALQUER valor é bem-vindo!
Se tiver interesse, por favor, me avise e eu passo os dados da conta bancária da Abadia São Geraldo, onde estamos concentrando todas as doações! 

Muito obrigada desde já!! 
Juntos transformaremos o Natal e o ano de muitas famílias!
🌟🎄🎅🏻🎁`;
  $: mensagem = value.replace(/\{nome\}/g, $contatos[0]?.nome ?? "NOME");
</script>

<style>
    pre {
      white-space: pre-line;
      background: none;
    }
  </style>
  

<label
  >Editar Mensagem
  <textarea name="mensagem" bind:value />
  <small>Use {"{nome}"} para aplicar o nome da pessoa</small>
</label>

{#if $contatos[0]}
  <article>
    <h4>Previsão da mensagem</h4>
    <pre>
        {mensagem}
    </pre>
  </article>
{/if}

<table role="grid">
  <thead>
    <tr>
      <th>Nome</th>
      <th>Contato</th>
      <th>Já foi enviado</th>
      <th>Enviar</th>
    </tr>
  </thead>
  <tbody>
    {#each $contatos as item}
      <tr>
        <td>{item.nome}</td>
        <td>{item.contato}</td>
        <td>
          <button
            on:click={() => toggleItem(item.contato)}
            class="outline"
            style="border: none; margin-bottom:0px; padding:1px; width:auto"
          >
            {#if item.status}
              ✅
            {:else}
              ❌
            {/if}
          </button>
        </td>
        <td>
          {#key value}
            <a role="button" target="_blank" href={getWhatsAppLink(item)}
              >Enviar Mensagem</a
            >
          {/key}
        </td>
      </tr>
    {/each}
  </tbody>
</table>
