# Publicação do site

## Nível escolhido
Nível 2 - Intermédio

## Rota escolhida
Nginx

*Justificação: o Nginx não estava instalado por omissão neste ambiente (WSL2 -
Ubuntu 22.04), à semelhança do que se verificou com o `openssh-server` no Tópico 2.
Foi escolhido por ser leve, amplamente usado em VPS reais (incluindo as sugeridas
nas ferramentas do módulo) e por ter uma configuração inicial simples para servir
ficheiros estáticos.*

## Ficheiros criados
- `site/index.html`
- `site/sobre.html`
- `site/style.css`

## Local de publicação
`/var/www/html/topico-03/` (subpasta dedicada dentro do diretório de publicação por
omissão do Nginx, para não sobrepor a página de boas-vindas original nem outros
tópicos futuros).

## Comandos principais utilizados
```
sudo apt update
sudo apt install -y nginx
sudo systemctl enable --now nginx
sudo mkdir -p /var/www/html/topico-03
sudo cp site/index.html site/sobre.html site/style.css /var/www/html/topico-03/
sudo chown -R www-data:www-data /var/www/html/topico-03
```
(Lista completa e finalidade de cada comando em `comandos.txt`.)

## Resultado obtido
Instalação e publicação concluídas com sucesso:
```
$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2026-09-24 12:44:32 -01; ...
   Main PID: 1414 (nginx)

$ ls -l /var/www/html/topico-03
-rw-r--r-- 1 www-data www-data 1360 Sep 24 12:44 index.html
-rw-r--r-- 1 www-data www-data 1093 Sep 24 12:44 sobre.html
-rw-r--r-- 1 www-data www-data  718 Sep 24 12:44 style.css
```
O serviço Nginx ficou ativo e persistente (`enable --now`), e os três ficheiros foram
copiados para `/var/www/html/topico-03/` com o dono/grupo `www-data` (o mesmo com que
o Nginx corre), em vez de ficarem associados ao utilizador pessoal `edmilson`.
Evidência completa em `evidencias/curl-validacao.txt`.

## Limitações encontradas
- O Nginx não vinha instalado por omissão nesta distribuição WSL2.
- A instalação exigiu privilégios de administrador (sudo); como o utilizador não
  recordava a password de sudo definida no WSL, a instalação e a publicação foram
  feitas com o utilizador `root` (`wsl -u root`), um mecanismo próprio do WSL que dá
  acesso administrativo à instância a partir da sessão Windows já autenticada, sem
  necessitar da password Linux. Não foi usada, partilhada nem armazenada nenhuma
  password para este efeito.
