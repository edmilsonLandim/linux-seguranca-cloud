# Trabalho de grupo - Tópico 4

## Grupo
*(preencher: nome/número do grupo, elementos e papéis assumidos — ver também a
Secção 1 do relatório)*

## Objetivo
Propor um plano de segurança inicial (superfície de ataque, firewall e
hardening) para o serviço web publicado no Tópico 3, sem ir ao nível de
hardening avançado.

## Cenário escolhido
**A — página HTML com Nginx**, coerente com o serviço realmente publicado e
protegido pelo grupo nos Tópicos 3 e 4 (não foi escolhido um cenário
hipotético diferente do trabalho já realizado).

## Produto
[`grupo-X-seguranca-hardening-topico-04.pdf`](grupo-X-seguranca-hardening-topico-04.pdf)
— relatório com 2 páginas, cobrindo: caracterização do serviço, superfície de
ataque, regras de firewall propostas, medidas de hardening inicial e plano de
validação.

## Evidências
Pasta `evidencias/` — reaproveita os outputs reais já recolhidos na atividade
individual do Tópico 4 (e do Tópico 2, para a ausência de SSH):
- `portas-antes.txt` — serviços/portas em escuta.
- `firewall-depois.txt` — estado do UFW e validação do serviço após ativação.
- `curl-validacao.txt` — validação HTTP do serviço publicado.
- `ssh-verificacao.txt` — confirmação de que não há servidor SSH instalado.

## Link do repositório GitHub
https://github.com/edmilsonLandim/linux-seguranca-cloud/tree/main/topico-04/trabalho-grupo
