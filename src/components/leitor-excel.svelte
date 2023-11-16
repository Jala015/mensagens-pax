<script>
  import * as XLSX from "xlsx";
  import { contatos } from "@scripts/store";

  let phones = [];
  let files;

  const handleFileChange = async () => {
    const file = files[0] ?? null;

    if (file) {
      const reader = new FileReader();

      reader.onload = (e) => {
        const data = new Uint8Array(e.target.result);
        const workbook = XLSX.read(data, { type: "array" });

        // Supondo que o nome da folha seja 'Sheet1'
        const sheetName = workbook.SheetNames[0];
        const sheet = workbook.Sheets[sheetName];

        // Obtendo os telefones da coluna "contato"
        const linhas = XLSX.utils.sheet_to_json(sheet);

        phones = linhas.map((linha) => ({
          nome: linha.nome,
          contato: linha.contato,
          status: false,
        }));

        contatos.set(phones);
      };

      reader.readAsArrayBuffer(file);
    }
  };
</script>

<div>
  <input bind:files type="file" accept=".xlsx, .xls" />
  <button on:click|preventDefault={handleFileChange}>Enviar</button>
</div>

{#if $contatos.length}
<a href="/lista-de-contatos" role="button">Próximo</a>
{/if}
