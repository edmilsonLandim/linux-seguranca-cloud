# Atividade prática individual - Tópico 4

## Nível realizado
Nível 2 - Intermédio (com base no Nível 1 como preparação obrigatória)

## Objetivo
Aplicar medidas iniciais de segurança ao serviço web publicado no Tópico 3:
identificar serviços e portas, compreender a superfície de ataque, aplicar
firewall (UFW) e validar que o serviço continua acessível.

## Ambiente utilizado
VM local (WSL2 - Ubuntu 22.04.3 LTS), o mesmo ambiente documentado desde o
Tópico 1.

## Resumo do trabalho
- Identificados os serviços em escuta com `lsof`/`ss`: apenas Nginx (porta 80)
  e `systemd-resolved` (porta 53, só em loopback) — sem SSH nem base de dados
  ativos.
- Identificados 5 riscos iniciais (ausência de firewall, divulgação da versão
  do Nginx, tráfego sem HTTPS, sistema por atualizar, e a particularidade do
  acesso root sem password neste ambiente WSL2).
- Aplicado UFW: política "deny incoming" por omissão, com exceção explícita
  para `80/tcp` (IPv4 e IPv6). SSH e HTTPS não foram abertos por não haver
  serviço correspondente ativo.
- Validado com `curl` que o serviço do Tópico 3 continua a responder HTTP 200
  depois do firewall ativo.
- Documentadas em `hardening.md`, como preparação para tópicos seguintes, as
  medidas de hardening específicas do Nginx que ainda não foram aplicadas e
  porquê.

## Ficheiros criados
- `superficie-ataque.md`, `firewall.md`, `hardening.md`, `validacao.md`
- `comandos.txt`
- `evidencias/portas-antes.txt`, `evidencias/firewall-depois.txt`

## URLs testados
`http://localhost/topico-03/index.html` e `http://localhost/topico-03/sobre.html`
— ambos com HTTP 200 antes e depois de ativar o firewall.

## Evidências produzidas
- `evidencias/portas-antes.txt` — inventário de serviços/portas antes de
  qualquer alteração.
- `evidencias/firewall-depois.txt` — estado final do UFW e resultado da
  validação `curl`.

## Dificuldades encontradas
Nenhuma dificuldade técnica relevante na aplicação do UFW em si. A principal
limitação foi de âmbito: por se tratar de um ambiente local (WSL2, sem IP
público e sem SSH instalado), algumas medidas do enunciado (permitir SSH,
validar acesso remoto real) não se aplicam neste momento - foram documentadas
como não aplicáveis, com justificação, em vez de simuladas.

## Link do repositório GitHub
https://github.com/edmilsonLandim/linux-seguranca-cloud/tree/main/topico-04/atividade-individual

## Próximos passos
Para o Tópico 5 (Gestão de Vulnerabilidades e Riscos): correr uma auditoria
com Lynis para validar objetivamente o estado atual (firewall, permissões,
atualizações) e priorizar as medidas de hardening já identificadas em
`hardening.md` por probabilidade e impacto. Para tópicos de monitorização:
começar a recolher os logs do Nginx (`access.log`/`error.log`) e do próprio
UFW (`/var/log/ufw.log`) como base de análise de eventos.
