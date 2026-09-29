# Monitorização

## Tempo de atividade (uptime)
```
$ uptime
10:30:44 up 4 min, 2 users, load average: 0.05, 0.11, 0.05
```
A instância estava ligada há apenas 4 minutos no momento da recolha - a
máquina WSL2 tinha sido reiniciada pouco antes (provavelmente por o Windows
anfitrião ter sido reiniciado ou a instância WSL ter sido encerrada entre
sessões de trabalho). Isto por si só já é um dado de monitorização relevante:
qualquer serviço com estado em memória (não é o caso do Nginx, mas seria o de
uma base de dados, por exemplo) teria sido reiniciado neste momento.

## Memória
```
$ free -h
              total   used   free   shared  buff/cache  available
Mem:          7.6Gi   442Mi  6.5Gi  3.0Mi    645Mi       7.0Gi
Swap:         2.0Gi   0B     2.0Gi
```
Uso de memória baixo (442Mi de 7.6Gi), sem uso de swap - sem sinais de
sobrecarga.

## Espaço em disco
```
$ df -h /
Filesystem   Size  Used Avail Use% Mounted on
/dev/sdd     1007G  2.5G  954G   1%  /
```
Apenas 1% do disco usado - sem risco de falha por espaço insuficiente no
curto/médio prazo.

## Estado do serviço web
```
$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Active: active (running) since Tue 2026-09-29 10:27:44 -01; 2min 59s ago
```
Nginx ativo e a correr, coerente com o horário de arranque do sistema (ligou-se
automaticamente no boot, como configurado no Tópico 3 com `systemctl enable`).

## Estado do firewall
```
$ sudo ufw status verbose
Status: active
Default: deny (incoming), allow (outgoing), disabled (routed)
80/tcp  ALLOW IN  Anywhere
80/tcp (v6)  ALLOW IN  Anywhere (v6)
```
Confirmação de que a política aplicada no Tópico 4 se mantém inalterada após o
reinício do sistema (as regras do UFW persistem entre reinícios, por terem
sido ativadas com `ufw enable`, que a torna permanente).

Evidência completa em `evidencias/monitorizacao.txt`.
