# Teste de API Automático com Postman, Newman e GitHub Actions
Este repositório contém uma automação de testes de API utilizando Postman, Newman e GitHub Actions para rodar automaticamente os testes diariamente.

Para verificar os testes e o HTML do dia anterior é:
https://luanfreitasqa.github.io/testes-south-newman/resultado.html

## Estrutura do Projeto
- **TesteSouth.postman_collection.json**: Arquivo da collection do Postman, que contém todos os endpoints e testes automatizados.
- **.github/workflows/newman.yml**: Arquivo de configuração do GitHub Actions que executa os testes com o Newman diariamente.

## Ferramentas Utilizadas
- **Postman**: Utilizado para criar e rodar os testes de API manualmente.
- **Newman**: Utilizado para rodar as collections do Postman via linha de comando.
- **Newman Reporter Htmlextra**: Gera relatórios HTML mais detalhados.
- **GitHub Actions**: Plataforma de CI/CD que executa automaticamente os testes da collection Postman.

## Como Rodar os Testes Localmente
### Pré-requisitos:
- **Node.js** (versão 16 ou superior)
- **Newman** instalado globalmente:
  ```bash
  npm install -g newman newman-reporter-htmlextra
  ```

### Executar a Collection Localmente
Para rodar a collection localmente com o Newman, execute o seguinte comando:

```bash
newman run "TesteSouth.postman_collection.json" -n 50 -r htmlextra --reporter-htmlextra-export resultado-bonito.html
```
Este comando executa a collection 50 vezes e gera um relatório HTML chamado `resultado-bonito.html`.

## Configuração do GitHub Actions
A automação no GitHub Actions foi configurada para rodar automaticamente todos os dias à meia-noite (UTC), utilizando o arquivo `newman.yml` dentro da pasta `.github/workflows`.

### Estrutura do Arquivo `newman.yml`
```yaml
name: Run Newman Collection Daily

on:
  workflow_dispatch:  # Permite rodar o workflow manualmente
  schedule:
    - cron: '0 0 * * *'  # Roda todos os dias à meia-noite (UTC)

jobs:
  newman-tests:
    runs-on: ubuntu-latest

    steps:
    # Verifica o código no repositório
    - name: Check out repository
      uses: actions/checkout@v3

    # Instala o Node.js (versão 16 ou superior)
    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '16'

    # Instala o Newman e o reporter htmlextra
    - name: Install Newman and Reporter
      run: npm install -g newman newman-reporter-htmlextra

    # Roda a Collection com 50 iterações e gera o relatório HTML
    - name: Run Newman Collection
      run: newman run "TesteSouth.postman_collection.json" -n 50 -r htmlextra --reporter-htmlextra-export resultado-bonito.html

    # Upload do relatório gerado como artefato
    - name: Upload Newman Report
      uses: actions/upload-artifact@v3
      with:
        name: newman-report
        path: resultado-bonito.html
```

## Como Funciona o Workflow
- **Check out do repositório**: O código da collection é baixado.
- **Instalação do Node.js**: Configura a versão 16 do Node.js.
- **Instalação do Newman e Reporter**: Instala o Newman e o Reporter Htmlextra.
- **Execução dos testes**: O Newman executa a collection `TesteSouth.postman_collection.json` 50 vezes e gera um relatório HTML.
- **Upload do Relatório**: O relatório gerado (`resultado-bonito.html`) é enviado como artefato.

## Visualização do Relatório
Após a execução do workflow no GitHub Actions, você pode baixar o relatório gerado como artefato diretamente na página de execução do workflow.

## Rodando os Testes Manualmente
Caso queira rodar os testes manualmente no GitHub Actions, siga estas etapas:

1. Vá até a aba **Actions** no repositório do GitHub.
2. Selecione o workflow **Run Newman Collection Daily**.
3. Clique no botão **Run Workflow** para rodar os testes manualmente.
