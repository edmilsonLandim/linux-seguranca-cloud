# Plano de continuidade operacional

*Esta atividade foi realizada ao Nível 2 (Intermédio - backup e recuperação
simples). Este ficheiro não é uma implementação de Nível 3 completa, mas o
plano de continuidade correspondente, com base no que já foi testado acima.*

## Serviço crítico
Nginx, servindo o conteúdo do Tópico 3 em `/var/www/html/topico-03/`.

## Ficheiros/configurações críticas
- `/var/www/html/topico-03/` — conteúdo publicado (já com backup testado).
- `/etc/nginx/sites-available/` e `/etc/nginx/nginx.conf` — configuração do
  servidor web (ainda não incluída no backup atual; ver "próximos passos").
- `linux-seguranca-cloud/` (todo o repositório) — já protegido por estar
  sincronizado com o GitHub, que funciona como backup remoto de facto.

## Logs importantes
- `/var/log/nginx/access.log` e `/var/log/nginx/error.log` — padrões de
  acesso e erros do serviço web.
- Saída de `journalctl -p err` — eventos de sistema a nível de sistema
  operativo (ex.: falhas de arranque de serviços).
- `sudo ufw status verbose` — não é um log per se, mas deve ser reconfirmado
  periodicamente para detetar alterações não documentadas às regras de
  firewall.

## Periodicidade de backup proposta
- **Conteúdo do site (`/var/www/html/topico-03`):** backup manual a cada
  alteração de conteúdo (baixo volume de alterações neste projeto). Numa VPS
  em produção real, o razoável seria um backup diário automatizado (ex.: via
  `cron` + `tar`, com rotação de versões antigas).
- **Configuração do Nginx:** incluir no mesmo backup a partir do próximo
  tópico, já que atualmente só o conteúdo é coberto.

## Procedimento de recuperação
1. Confirmar qual o backup mais recente íntegro (`tar -tzf` sobre o ficheiro,
   sem extrair, para validar que não está corrompido).
2. Extrair para uma pasta de teste isolada (nunca diretamente sobre
   `/var/www/html`), exatamente como feito em `backup-recuperacao.md`.
3. Validar o conteúdo restaurado (`diff -r` e `md5sum` contra a última cópia
   conhecida boa, quando existir).
4. Só depois de validado, copiar para `/var/www/html/topico-03/` e repor
   `chown -R www-data:www-data`.
5. Validar o serviço com `curl` (como feito nos Tópicos 3 e 4), confirmando
   HTTP 200 nas páginas principais.

## Critérios de validação após recuperação
- Os ficheiros restaurados têm checksum MD5 idêntico ao backup de origem.
- O dono/grupo dos ficheiros restaurados é `www-data:www-data` (não o
  utilizador pessoal).
- `curl -I` às páginas principais devolve HTTP 200.
- `systemctl status nginx` mostra o serviço `active (running)` sem reiniciar
  inesperadamente após a recuperação.

## O que fica para tópicos seguintes
- Automatizar o backup (`cron`) em vez de o correr manualmente.
- Incluir a configuração do Nginx no backup, não só o conteúdo.
- Definir uma política de retenção (quantas versões de backup manter).
- Testar um cenário de recuperação a partir de uma cópia fora da própria
  máquina (ex.: o próprio GitHub, ou um destino de backup externo), e não só
  localmente, como feito aqui.
