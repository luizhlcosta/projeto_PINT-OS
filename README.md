# Pint-OS — Alarm Clock

Implementação da atividade "Alarm Clock" da disciplina de Infraestrutura de Software (CESAR School), baseada no Pint-OS (Stanford).

## O que foi feito

Reimplementação de `timer_sleep()` em `devices/timer.c` para eliminar o busy wait original. A nova versão:

- Bloqueia a thread chamadora (`thread_block()`) em vez de mantê-la girando em `thread_yield()`.
- Registra o tick de despertar (`wakeup_tick`) da thread em uma lista global ordenada (`wakeup_list`).
- No handler `timer_interrupt()`, verifica a cada tick se alguma thread da lista já deve acordar e a desbloqueia (`thread_unblock()`).

## Arquivos modificados

- `src/devices/timer.c`
- `src/threads/thread.h` (novo campo `wakeup_tick` em `struct thread`)

## Testes

Rodados via `make check` no diretório `threads`. Resultado resumido:

| Teste | Resultado |
|---|---|
| alarm-single | ✅ pass |
| alarm-multiple | ✅ pass |
| alarm-simultaneous | ✅ pass |
| alarm-zero | ✅ pass |
| alarm-negative | ✅ pass |
| alarm-priority | ❌ fail (depende de priority scheduling, fora do escopo desta atividade) |

Saída completa em [`evidencias.log`](src/threads/evidencias.log).

## Como compilar e testar

```bash
cd src/threads
make
cd build
make check
```

## Observações

Testes de `priority-*` e `mlfqs-*` falham propositalmente — dependem de funcionalidades de escalonamento por prioridade não implementadas nesta atividade.