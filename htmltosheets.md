# Guia de Implementação para Integração do Quiz com Google Sheets (Com Prevenção de Duplicatas)

Este guia mostrará como configurar o quiz para enviar as respostas para uma planilha do Google.

## 1. Criar uma nova planilha no Google Sheets

1. Acesse [Google Sheets](https://sheets.google.com) e crie uma nova planilha
2. Renomeie a planilha para "Respostas do Quiz Feng Shui" ou outro nome de sua preferência
3. Anote o ID da planilha (a longa sequência de caracteres na URL entre `/d/` e `/edit`)

## 2. Configurar o Google Apps Script

1. Na planilha, clique em **Extensões** > **Apps Script**
2. Será aberto o editor do Google Apps Script
3. Renomeie o projeto (canto superior esquerdo) para "Quiz Feng Shui"
4. Cole o código do arquivo `google-apps-script.js` no editor, substituindo todo o conteúdo existente
5. Na linha que contém `const spreadsheetId = 'SEU_ID_DA_PLANILHA_AQUI';`, substitua `SEU_ID_DA_PLANILHA_AQUI` pelo ID da planilha que você anotou anteriormente
6. Salve o projeto clicando em **Salvar** (ícone de disquete)

## 3. Implantar o Web App

1. No editor do Apps Script, clique em **Implantar** > **Novo Deployment**
2. Em "Tipo de implantação", selecione **Web app**
3. Configure as opções:
   - Descrição: "Quiz Feng Shui Web App"
   - Execute como: **Eu** (seu e-mail)
   - Quem tem acesso: **Qualquer pessoa**
4. Clique em **Implantar**
5. Autorize o aplicativo quando solicitado
6. Após a implantação, copie a URL do Web App que é exibida

## 4. Atualizar o código HTML do quiz

1. Abra o arquivo `quiz-google-sheets.html`
2. Localize a linha que contém:
   ```javascript
   const scriptURL = 'COLOQUE_SUA_URL_DO_SCRIPT_WEB_APP_AQUI';
   ```
3. Substitua `COLOQUE_SUA_URL_DO_SCRIPT_WEB_APP_AQUI` pela URL do Web App que você copiou
4. Salve o arquivo

## 5. Testar a integração

1. Abra o arquivo HTML modificado em um navegador
2. Preencha o formulário com informações de teste
3. Envie o formulário
4. Verifique a planilha do Google para confirmar se os dados foram recebidos

## Notas importantes

- **Segurança**: O Web App está configurado para ser acessível por qualquer pessoa. Isso é necessário para que o formulário possa enviar dados sem autenticação, mas significa que qualquer pessoa com a URL pode enviar dados para sua planilha.
- **Limites de uso**: O Google Apps Script tem [limites de uso](https://developers.google.com/apps-script/guides/services/quotas) que podem afetar seu aplicativo se ele receber muitos envios em um curto período.
- **Atualizações**: Se você modificar o script após a implantação, será necessário criar uma nova versão para que as alterações entrem em vigor.
- **Prevenção de duplicatas**: O sistema agora inclui mecanismos tanto no cliente quanto no servidor para prevenir o envio de dados duplicados:
  - No cliente: impede que o usuário submeta o mesmo formulário várias vezes em sequência
  - No servidor: verifica se os dados já foram enviados anteriormente usando um sistema de checksums
  - Uma aba oculta chamada "Checksums" é criada automaticamente para gerenciar este controle

## Personalizações adicionais

### Para adicionar mais campos ao formulário:

1. Adicione os novos campos HTML no arquivo `quiz-google-sheets.html` dentro da div `form-header`
2. Atualize o objeto `personalInfo` no script para incluir os novos campos
3. Atualize o código do Google Apps Script para capturar e armazenar os novos campos
4. Adicione os novos campos aos cabeçalhos da planilha

### Para coletar dados adicionais sobre as respostas:

1. Modifique o objeto `formData` para incluir as informações adicionais
2. Atualize o script do Google Apps Script para processar essas informações
