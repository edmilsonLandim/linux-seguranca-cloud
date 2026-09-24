# Atividade prática individual - Tópico 3

## Nível realizado
Nível 2 - Intermédio

## Objetivo
Criar e publicar um serviço na web em ambiente Linux.

## Ambiente utilizado
VM local (WSL2 - Ubuntu 22.04.3 LTS), o mesmo ambiente documentado desde o Tópico 1.

## Rota de publicação
Nginx

## Ficheiros criados
- `site/index.html`
- `site/sobre.html`
- `site/style.css`
- `comandos.txt`, `publicacao.md`, `validacao.md`, `README.md` (documentação)

## URLs testados
`http://localhost/topico-03/index.html` e `http://localhost/topico-03/sobre.html`
— **PENDENTE**: resultado real a confirmar após a instalação do Nginx (ver
`publicacao.md` e `validacao.md`).

## Evidências produzidas
- `evidencias/site-criado.txt` (estrutura do site antes da publicação)
- `evidencias/curl-validacao.txt` — **a criar** após a instalação do Nginx

## Dificuldades encontradas
O Nginx não vem instalado por omissão no ambiente WSL2 usado. A instalação exige
privilégios de administrador (sudo com password interativa), pelo que foi executada
manualmente pelo utilizador, fora da automação usada para o resto da atividade —
mesma limitação de fundo já documentada no Tópico 2 a propósito do SSH.

## Link do repositório GitHub
*(a preencher)*

## Próximos passos
Para o Tópico 4 (Monitorização e Logging) e para o reforço de segurança que se segue
a este tópico, importa: restringir o acesso ao servidor (firewall/portas), rever
permissões do diretório `/var/www/html/topico-03` (garantir que só `www-data` tem
escrita), remover ou desativar a página de boas-vindas por omissão do Nginx, e
começar a recolher os registos de acesso (`access.log`/`error.log`) do próprio Nginx
como base para a monitorização.
