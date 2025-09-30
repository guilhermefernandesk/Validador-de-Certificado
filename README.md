# Validador-de-Certificado

Este projeto é um validador de certificados simples, implementado com HTML, CSS e JavaScript, que se conecta a um backend de validação construído com Google Apps Script. O objetivo é permitir que os usuários insiram um código de autenticidade e verifiquem se o certificado é válido, exibindo os detalhes correspondentes.

## Como Configurar o Backend com Google Apps Script

O backend para este validador é um Google Apps Script que lê dados de uma Google Sheet. Siga os passos abaixo para configurá-lo.

### 1. Crie uma Google Sheet para os Certificados

Primeiro, você precisará de uma planilha Google onde seus dados de certificado serão armazenados.

1.  Vá para [Google Sheets](https://docs.google.com/spreadsheets/u/0/) e crie uma nova planilha.
2.  Na primeira linha (cabeçalho), adicione as seguintes colunas (exatamente como escrito):
    - `ID` (para o código de autenticidade do certificado)
    - `Nome` (nome do titular do certificado)
    - `Curso` (nome do evento ou curso)
    - `Emitido Em` (data de emissão do certificado)
3.  Preencha algumas linhas com dados de exemplo.

    **Exemplo de Planilha `Banco de Dados de Validação`:**

    | ID       | Nome           | Curso                      | Emitido Em |
    | :------- | :------------- | :------------------------- | :--------- |
    | ABC12345 | João da Silva  | Introdução à Programação   | 2023-01-15 |
    | DEF67890 | Maria Oliveira | Desenvolvimento Web Básico | 2023-03-20 |
    | GHI11223 | Pedro Souza    | Banco de Dados SQL         | 2023-05-10 |

4.  Clique em Extensões > Apps Scripts

### 2. Em Google Apps Script

1.  Renomeie o projeto para algo como `Validador de Certificado`.

2.  No editor de código do Google Apps Script (o arquivo `Code.gs`), substitua o conteúdo existente pelo código abaixo:

### 3. Implante o Script como um Aplicativo Web

1.  No editor do Apps Script, clique em `Implantar` (Deploy) no canto superior direito e selecione `Nova implantação` (New deployment).
2.  Clique no ícone de engrenagem (`⚙️`) ao lado de "Selecionar tipo" e escolha `Aplicativo da Web` (Web app).
3.  Configure a implantação:
    - **Descrição da implantação:** (Opcional) `Validador de Certificado`
    - **Executar como:** `Eu` (seu e-mail)
    - **Quem tem acesso:** `Qualquer pessoa` (Anyone) - Isso é crucial para que seu frontend possa acessá-lo.
4.  Clique em `Implantar`.
5.  Você será solicitado a autorizar o script a acessar seus dados do Google Sheets. Siga as instruções para conceder as permissões necessárias.
6.  Após a implantação bem-sucedida, você receberá uma `URL do aplicativo da Web` (Web app URL). Copie esta URL.

### 4. Atualize o `index.html` com a URL do seu Aplicativo Web

Agora que você tem a URL do seu Google Apps Script implantado, você precisa atualizar o arquivo `index.html` do seu projeto frontend.

1.  Abra o arquivo

2.  Substitua a URL existente pela `URL do aplicativo da Web` que você copiou no passo anterior.

    Exemplo:

    ```javascript
    const webAppUrl = "SUA_NOVA_URL_DO_APLICATIVO_WEB_AQUI";
    ```

Agora, seu validador de certificado está configurado e pronto para uso! Abra o `index.html` no seu navegador e teste com os IDs de certificado da sua Google Sheet.
