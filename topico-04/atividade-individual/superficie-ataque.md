# Superfície de ataque

## Serviço web usado no Tópico 3
Nginx 1.18.0, publicando o portefólio técnico em `/var/www/html/topico-03/`
(ver Tópico 3 para detalhes de publicação).

## Serviços ativos (antes do firewall)
```
$ sudo lsof -i -n -P | grep LISTEN
systemd-r 133 systemd-resolve  TCP 127.0.0.53:53 (LISTEN)
nginx     244 root             TCP *:80 (LISTEN)   [IPv4 e IPv6]
nginx     245-252  www-data    TCP *:80 (LISTEN)   [8 worker processes]
```
Resumo: apenas dois serviços distintos têm sockets em escuta nesta instância:
- **nginx** — servidor web, na porta 80 (o processo master corre como `root`,
  mas os *workers* que efetivamente atendem pedidos correm como `www-data`,
  utilizador sem privilégios).
- **systemd-resolved** — resolução de DNS local, na porta 53, exclusivamente em
  `127.0.0.53` (loopback) - não está exposto à rede.

Não há SSH (`sshd`) nem base de dados (ex.: MySQL/MariaDB na porta 3306) ativos
nesta instância - confirmado também no Tópico 2, a propósito do SSH.

## Portas abertas ou observadas
| Porta | Protocolo | Serviço | Alcance |
|---|---|---|---|
| 80 | TCP | Nginx (HTTP) | Todas as interfaces (IPv4 `*:80` e IPv6 `[::]:80`) |
| 53 | TCP/UDP | systemd-resolved (DNS) | Apenas `127.0.0.53` (loopback) |
| 443 | TCP | HTTPS | **Não usado** - Nginx não tem certificado TLS configurado |
| 22 | TCP | SSH | **Não usado** - `openssh-server` não está instalado (Tópico 2) |
| 3306 | TCP | Base de dados | **Não usado** - não existe nenhuma base de dados instalada |

## Portas necessárias
Apenas a **porta 80 (HTTP)** é necessária neste momento, porque é a única forma
de aceder ao serviço web publicado no Tópico 3. A porta 53 não precisa de regra
de firewall porque só escuta em loopback (não é alcançável de fora da própria
máquina). As restantes portas (443, 22, 3306) não estão associadas a nenhum
serviço ativo, logo não há necessidade de as abrir - abri-las "preventivamente"
aumentaria a superfície de ataque sem qualquer benefício.

## Riscos iniciais identificados
1. **Ausência de firewall ativo (antes desta atividade).** Sem UFW ativado,
   qualquer porta que venha a ficar em escuta (por exemplo, por instalação
   futura de outro serviço) fica imediatamente acessível sem controlo
   explícito - o risco não é só a porta 80 atual, é a ausência de uma política
   "negar por omissão".
2. **Divulgação de informação do servidor (information disclosure).** Por
   omissão, o Nginx responde ao cabeçalho `Server` com a versão exata
   (`nginx/1.18.0 (Ubuntu)`), visível em qualquer pedido `curl -I` - informação
   que facilita a um atacante procurar vulnerabilidades conhecidas dessa versão
   específica.
3. **Tráfego HTTP em texto simples (sem HTTPS).** Sem certificado TLS
   configurado, todo o tráfego entre cliente e servidor (incluindo eventuais
   formulários ou dados no futuro) circula sem cifra, sujeito a interceção.
4. **Sistema sem atualização confirmada.** Desde a instalação do Nginx no
   Tópico 3, não foi executado `apt update && apt upgrade` - pacotes podem já
   ter correções de segurança disponíveis por aplicar.
5. **Acesso administrativo por `wsl -u root`, sem password.** Neste ambiente
   específico (WSL2), qualquer processo que corra na sessão Windows do
   utilizador consegue obter acesso root à instância Linux sem password - uma
   particularidade do modelo de confiança do WSL que não existe (nem deve
   existir) numa VPS real, onde o acesso root exige sempre autenticação
   própria.

## Relação falha / ameaça / risco
- **Falha (vulnerabilidade):** ausência de firewall ativo e divulgação da
  versão do Nginx nos cabeçalhos HTTP.
- **Ameaça:** um atacante a fazer scanning de portas ou a explorar
  vulnerabilidades conhecidas de versões específicas de software.
- **Risco:** probabilidade de exploração baixa neste ambiente local (sem IP
  público), mas seria alta se este mesmo estado fosse replicado numa VPS com
  IP público - por isso as medidas abaixo (firewall) foram aplicadas já, antes
  de qualquer exposição real à internet.
