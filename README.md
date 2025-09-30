# Validador-de-Certificado

Este projeto é um validador de certificados simples, implementado com HTML, CSS e JavaScript, que se conecta a um backend de validação construído com Google Apps Script. O objetivo é permitir que os usuários insiram um código de autenticidade e verifiquem se o certificado é válido, exibindo os detalhes correspondentes.

## Como Configurar o Backend com Google Apps Script

O backend para este validador é um Google Apps Script que lê dados de uma Google Sheet. Siga os passos abaixo para configurá-lo.

### 1. Crie uma Google Sheet para os Certificados

Primeiro, você precisará de uma planilha Google onde seus dados de certificado serão armazenados.

1.  Vá para [Google Sheets](https://docs.google.com/spreadsheets/u/0/) e crie uma nova planilha.
2.  Renomeie a primeira aba (sheet) para `Certificados`.
3.  Na primeira linha (cabeçalho), adicione as seguintes colunas (exatamente como escrito):
    - `ID` (para o código de autenticidade do certificado)
    - `Nome` (nome do titular do certificado)
    - `Curso` (nome do evento ou curso)
    - `Emitido Em` (data de emissão do certificado)
4.  Preencha algumas linhas com dados de exemplo.

    **Exemplo de Planilha `Certificados`:**

    | ID       | Nome           | Curso                      | Emitido Em |
    | :------- | :------------- | :------------------------- | :--------- |
    | ABC12345 | João da Silva  | Introdução à Programação   | 2023-01-15 |
    | DEF67890 | Maria Oliveira | Desenvolvimento Web Básico | 2023-03-20 |
    | GHI11223 | Pedro Souza    | Banco de Dados SQL         | 2023-05-10 |

5.  **Obtenha o ID da sua Google Sheet:** O ID da planilha é uma longa sequência de caracteres na URL da sua planilha, entre `/d/` e `/edit`.
    Ex: `https://docs.google.com/spreadsheets/d/YOUR_SPREADSHEET_ID_HERE/edit#gid=0`
    Anote este ID, você precisará dele no próximo passo.

### 2. Crie um Projeto Google Apps Script

1.  Vá para Google Apps Script.
2.  Clique em `Novo projeto`.
3.  Renomeie o projeto para algo como `ValidadorDeCertificadoBackend`.

### 3. Implemente o Código do Script

No editor de código do Google Apps Script (o arquivo `Code.gs`), substitua o conteúdo existente pelo código abaixo:

```javascript
// Função auxiliar para formatar a resposta como JSON.
function createJsonResponse(data) {
  return ContentService.createTextOutput(JSON.stringify(data)).setMimeType(ContentService.MimeType.JSON);
}

function doGet(e) {
  try {
    const certificateId = e.parameter.id;
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0]; // pega primeira aba
    const data = sheet.getDataRange().getValues() || [];

    // Remove a linha do cabeçalho (linha 0) para não incluí-la na busca.
    const records = data.slice(1);

    // Procura pela linha onde a primeira coluna (índice 0) corresponde ao ID do certificado.
    const foundRow = records.find((row) => row[0] === certificateId);

    // Se a linha for encontrada...
    if (foundRow) {
      // Monta o objeto de resposta com os dados do certificado.
      const certificateData = {
        status: "válido",
        nome: foundRow[1], // Coluna B: Nome do Aluno
        curso: foundRow[2], // Coluna C: Nome do Evento
        emitido_em: foundRow[3], // Coluna D: Emitido em
      };

      return createJsonResponse({
        valid: true,
        certificate: certificateData,
      });
    } else {
      // Se não encontrar, retorna que o certificado é inválido.
      return createJsonResponse({
        valid: false,
        message: "Certificado inválido ou não encontrado.",
      });
    }
  } catch (error) {
    return createJsonResponse({
      valid: false,
      message: `Ocorreu um erro interno no servidor de validação. ${error}`,
    });
  }
}
```

**Lembre-se de substituir `YOUR_SPREADSHEET_ID_HERE` pelo ID da sua Google Sheet!**

### 4. Implante o Script como um Aplicativo Web

1.  No editor do Apps Script, clique em `Implantar` (Deploy) no canto superior direito e selecione `Nova implantação` (New deployment).
2.  Clique no ícone de engrenagem (`⚙️`) ao lado de "Selecionar tipo" e escolha `Aplicativo da Web` (Web app).
3.  Configure a implantação:
    - **Descrição da implantação:** (Opcional) `Validador de Certificado`
    - **Executar como:** `Eu` (seu e-mail)
    - **Quem tem acesso:** `Qualquer pessoa` (Anyone) - Isso é crucial para que seu frontend possa acessá-lo.
4.  Clique em `Implantar`.
5.  Você será solicitado a autorizar o script a acessar seus dados do Google Sheets. Siga as instruções para conceder as permissões necessárias.
6.  Após a implantação bem-sucedida, você receberá uma `URL do aplicativo da Web` (Web app URL). Copie esta URL.

### 5. Atualize o `index.html` com a URL do seu Aplicativo Web

Agora que você tem a URL do seu Google Apps Script implantado, você precisa atualizar o arquivo `index.html` do seu projeto frontend.

1.  Abra o arquivo `c:\Users\guilh\Documents\programacao\Validador-de-Certificado\index.html`.
2.  Localize a linha que define `webAppUrl`:
    ```javascript
    const webAppUrl = "https://script.google.com/macros/s/AKfycbyOTL0ND2g3-U0Ed-VikVx5HGPyEk4LmDI8HAiOg2vKEes9WRxcLen7Rh0xNeduFfwyhQ/exec";
    ```
3.  Substitua a URL existente pela `URL do aplicativo da Web` que você copiou no passo anterior.

    Exemplo:

    ```javascript
    const webAppUrl = "SUA_NOVA_URL_DO_APLICATIVO_WEB_AQUI";
    ```

Agora, seu validador de certificado está configurado e pronto para uso! Abra o `index.html` no seu navegador e teste com os IDs de certificado da sua Google Sheet.

## Desenvolvimento Local

Para desenvolver e testar o frontend localmente, basta abrir o arquivo `index.html` diretamente no seu navegador. Ele fará as chamadas para o Google Apps Script implantado.

## Estrutura do Projeto

- `index.html`: Contém a interface do usuário (HTML, CSS e JavaScript) para o validador.
- `README.md`: Este arquivo, com instruções de configuração.

## Contribuições

Sinta-se à vontade para contribuir com melhorias ou correções.
