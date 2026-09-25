# Validação

## URLs testados
- `http://localhost/topico-03/index.html`
- `http://localhost/topico-03/sobre.html`

## Resultado dos testes
Ambas as páginas continuam a responder **HTTP 200** depois de o UFW ter sido
ativado com a política "deny incoming" por omissão e a regra explícita
`allow 80/tcp`:
```
$ curl -s -o /dev/null -w 'HTTP status: %{http_code}\n' http://localhost/topico-03/index.html
HTTP status: 200

$ curl -s -o /dev/null -w 'HTTP status: %{http_code}\n' http://localhost/topico-03/sobre.html
HTTP status: 200
```
Ou seja: o firewall está a bloquear tudo o que não foi explicitamente
permitido, e a única exceção (porta 80) continua a funcionar como esperado -
confirmação de que a regra foi bem aplicada e não há sobre-bloqueio nem
sub-bloqueio.

## Evidências
- `evidencias/portas-antes.txt` — inventário de serviços/portas em escuta
  antes de qualquer alteração (`lsof`/`ss`).
- `evidencias/firewall-depois.txt` — estado final do UFW (`ufw status verbose`)
  e o resultado dos dois testes `curl` acima, capturados na mesma execução.

## Observações
Tal como no Tópico 3, a validação foi feita via `localhost`, por se tratar de
um ambiente local (WSL2) sem IP público. O comportamento do UFW (política
"deny incoming" com exceção explícita) é o mesmo que seria aplicado numa VPS
real - a diferença está apenas na ausência de exposição pública neste ambiente.
