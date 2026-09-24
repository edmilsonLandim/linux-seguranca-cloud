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
`http://localhost/topico-03/index.html`, `http://localhost/topico-03/sobre.html` e
`http://localhost/topico-03/style.css` — todos confirmados com HTTP 200 (ver
`evidencias/curl-validacao.txt`).

## Evidências produzidas
- `evidencias/site-criado.txt` (estrutura do site antes da publicação)
- `evidencias/curl-validacao.txt` (estado do serviço Nginx, respostas HTTP e
  permissões finais dos ficheiros publicados)

## Dificuldades encontradas
O Nginx não vinha instalado por omissão no ambiente WSL2 usado, e a instalação exige
privilégios de administrador. Como a password de sudo do utilizador Linux não estava
disponível, a instalação e a publicação foram feitas via `wsl -u root` (mecanismo
próprio do WSL, sem necessidade dessa password) — mesma linha da limitação já
documentada no Tópico 2 a propósito do SSH e do princípio do menor privilégio.

## Link do repositório GitHub
https://github.com/edmilsonLandim/linux-seguranca-cloud

## Próximos passos
Para o Tópico 4 (Monitorização e Logging) e para o reforço de segurança que se segue
a este tópico, importa: restringir o acesso ao servidor (firewall/portas), rever
permissões do diretório `/var/www/html/topico-03` (garantir que só `www-data` tem
escrita), remover ou desativar a página de boas-vindas por omissão do Nginx, e
começar a recolher os registos de acesso (`access.log`/`error.log`) do próprio Nginx
como base para a monitorização.
