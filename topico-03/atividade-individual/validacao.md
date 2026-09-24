# Validação

## URLs testados
- `http://localhost/topico-03/index.html`
- `http://localhost/topico-03/sobre.html`

## Resultado dos testes
**PENDENTE** - a validação depende da instalação do Nginx (ver `publicacao.md`),
ainda por confirmar. Assim que o serviço estiver ativo, será executado:
```
curl -I http://localhost/topico-03/index.html
curl http://localhost/topico-03/sobre.html
```
e o output real será guardado em `evidencias/` e resumido aqui (código de estado
HTTP, confirmação do conteúdo das duas páginas e da aplicação do `style.css`).

## Evidências
- `evidencias/site-criado.txt` - confirma a criação dos três ficheiros do site
  (`ls -l`), antes da publicação.
- `evidencias/curl-validacao.txt` - **a criar** após a instalação do Nginx, com o
  output real dos comandos `curl` acima.

## Observações
Por se tratar de um ambiente local (WSL2), a validação será feita via `localhost`,
sem IP público - à semelhança da limitação já documentada no Tópico 2 em relação ao
SSH. A validação com IP público/domínio real ficará reservada para um cenário de
VPS, se vier a ser explorado em tópicos posteriores.
