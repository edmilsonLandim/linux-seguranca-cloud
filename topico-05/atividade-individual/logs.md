# Logs

## Logs do sistema (journalctl)
```
$ journalctl -p err -n 20 --no-pager
Sep 25 15:48:21 RAI-GV-NOSI-189 systemd[500]: user@1000.service: Failed to attach to
  cgroup /user.slice/user-1000.slice/user@1000.service: Device or resource busy
Sep 25 15:48:21 RAI-GV-NOSI-189 systemd[500]: user@1000.service: Failed at step CGROUP
  spawning /lib/systemd/systemd: Device or resource busy
Sep 25 15:48:21 RAI-GV-NOSI-189 systemd[1]: Failed to start User Manager for UID 1000.
-- Boot 531ef9e02e9546fd8faec3112634e250 --
Sep 29 10:27:44 RAI-GV-NOSI-189 kernel: PCI: Fatal: No config space access function found
Sep 29 10:27:44 RAI-GV-NOSI-189 kernel: misc dxg: dxgk: ... Ioctl failed: -22 (várias linhas)
```

## Logs de autenticação
Não existe `/var/log/auth.log` populado com tentativas de login remoto porque
não há servidor SSH instalado nesta instância (ver Tópico 2) - não há,
portanto, superfície de autenticação remota a auditar neste ambiente.

## Logs do serviço web (Nginx)
```
$ tail -10 /var/log/nginx/access.log
127.0.0.1 - - [29/Sep/2026:10:30:17 -0100] "GET /topico-03/index.html HTTP/1.1" 200 1360 "-" "curl/7.81.0"
127.0.0.1 - - [29/Sep/2026:10:30:17 -0100] "GET /topico-03/sobre.html HTTP/1.1" 200 1093 "-" "curl/7.81.0"
127.0.0.1 - - [29/Sep/2026:10:30:17 -0100] "GET /topico-03/nao-existe.html HTTP/1.1" 404 162 "-" "curl/7.81.0"

$ tail -10 /var/log/nginx/error.log
(vazio)
```

## Eventos relevantes identificados

1. **Falha no arranque do User Manager do utilizador `edmilson` (Sep 25
   15:48:21).** `systemd[500]: user@1000.service: Failed to attach to cgroup
   ... Device or resource busy`. Interpretação: ocorreu durante o encerramento
   da sessão WSL anterior (não durante o funcionamento normal) - é uma
   particularidade conhecida do WSL2 ao gerir cgroups de sessões de utilizador
   ao desligar, e não indica um problema no serviço web nem uma tentativa de
   intrusão. Não requer ação corretiva, mas fica registado como o tipo de
   evento a não confundir com um incidente de segurança.

2. **Pedido HTTP 404 a `/topico-03/nao-existe.html` (29/09/2026 10:30:17).**
   Neste caso foi gerado propositadamente (`curl`) para ter um evento real a
   analisar, mas ilustra exatamente o tipo de entrada de log que, em produção,
   merece atenção: um volume anómalo de pedidos 404 a caminhos diferentes,
   num curto espaço de tempo, é um padrão típico de reconhecimento automático
   (scanning) por parte de um atacante à procura de ficheiros ou rotas
   sensíveis (ex.: `/wp-admin`, `/.env`, `/admin`). Um único 404 isolado, como
   este, não é motivo de alarme.

## Relação com a monitorização cloud
Nesta instância local (WSL2) a monitorização é feita diretamente sobre os
logs do sistema e do Nginx. Numa VPS real, estes mesmos sinais (uso de
CPU/memória/disco, estado do serviço, padrões de acesso nos logs) seriam
tipicamente complementados por métricas e alertas da própria infraestrutura
cloud (ex.: painéis de monitorização do fornecedor, alertas por limite de
CPU/disco) - uma camada adicional que não existe neste ambiente local, mas que
seria o próximo passo natural ao migrar para uma VPS.

Evidência completa em `evidencias/logs.txt`.
