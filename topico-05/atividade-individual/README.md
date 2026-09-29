# Atividade prática individual - Tópico 5

## Nível realizado
Nível 2 - Intermédio (com o Nível 1 como preparação obrigatória)

## Objetivo
Acompanhar o estado do sistema, consultar registos, criar backup do conteúdo
crítico publicado no Tópico 3 e testar a sua recuperação.

## Ambiente utilizado
VM local (WSL2 - Ubuntu 22.04.3 LTS), o mesmo ambiente documentado desde o
Tópico 1.

## Resumo do trabalho
- **Monitorização:** sistema com apenas 4 minutos de atividade (reinício
  recente da instância), uso de memória e disco baixos, Nginx ativo e UFW com
  a política do Tópico 4 ainda em vigor.
- **Logs:** identificado um evento de sistema real (falha do User Manager do
  utilizador na sessão anterior, própria do WSL2, sem impacto de segurança) e
  gerado um evento de teste (pedido HTTP 404), documentando como interpretar
  cada um.
- **Backup:** criado backup comprimido de `/var/www/html/topico-03/`.
- **Recuperação:** restaurado numa pasta de teste isolada; `diff -r` sem
  diferenças e checksums MD5 idênticos aos originais - recuperação validada
  ao nível de byte, não apenas visualmente.
- **Continuidade (notas para o próximo nível):** plano documentado em
  `continuidade.md`, com periodicidade de backup proposta, procedimento de
  recuperação e critérios de validação, separando o que já está coberto do
  que fica para automatizar em tópicos seguintes.

## Ficheiros criados
- `monitorizacao.md`, `logs.md`, `backup-recuperacao.md`, `continuidade.md`
- `comandos.txt`
- `backups/backup-topico-03-20260929-103044.tar.gz`
- `evidencias/monitorizacao.txt`, `evidencias/logs.txt`,
  `evidencias/backup-criado.txt`, `evidencias/recuperacao-teste.txt`

## Evidências produzidas
Ver pasta `evidencias/` - inclui outputs reais de `uptime`, `free`, `df`,
`systemctl`, `ufw`, `journalctl`, logs do Nginx, criação do backup e teste de
recuperação com `diff`/`md5sum`.

## Dificuldades encontradas
Os logs do Nginx estavam vazios no início da sessão (instância recém-reiniciada,
sem tráfego ainda) - foi gerado tráfego real (incluindo um pedido inexistente,
HTTP 404) para ter eventos concretos a analisar, em vez de documentar com
dados simulados. Fora isso, sem dificuldades técnicas relevantes.

## Link do repositório GitHub
https://github.com/edmilsonLandim/linux-seguranca-cloud/tree/main/topico-05/atividade-individual

## Próximos passos
Para o Tópico 6 (Avaliação) e para consolidar o produto final do módulo:
automatizar o backup (`cron`), alargar o backup à configuração do Nginx (não
só ao conteúdo), e considerar um destino de backup fora da própria máquina -
o próprio repositório GitHub já cumpre parcialmente esse papel para o código e
documentação, mas não para os artefactos de backup binários gerados aqui.
