# Clyvo ELLV — Testes (Compliance, Quality Assurance & Tests) 🧪

Entrega da **Sprint 4** da disciplina **Compliance, Quality Assurance & Tests**, Challenge FIAP 2026 em parceria com a **Clyvo Vet**.

O **Clyvo ELLV** é um aplicativo mobile de gestão veterinária: cadastro de pacientes e responsáveis, histórico de consultas, lembretes no celular e alerta de possível surto por região. O app consome uma API REST própria, publicada na nuvem.

---

## 🔗 Links da entrega

| Item | Link |
|---|---|
| **Parte A — Plano de testes manuais (Azure Boards)** | https://dev.azure.com/RM561713/ELLV-Solu%C3%A7%C3%A3o-Mobile/_backlogs/backlog/ELLV-Solu%C3%A7%C3%A3o-Mobile%20Team/Epics |
| **Parte B — Vídeo da automação (configuração, execução e resultados)** | COLOQUE_AQUI_O_LINK_DO_VIDEO |
| API testada | https://api-clyvo-node.onrender.com |
| Documentação da API (Swagger) | https://api-clyvo-node.onrender.com/docs |

---

## 🤖 Parte B — Automação de testes (Postman)

Os testes automatizados validam a **API REST** que sustenta todas as telas do aplicativo: login, cadastro, pacientes e consultas. Foram feitos **11 casos de teste** (o mínimo pedido é 4), com **29 verificações**.

### Arquivos

| Arquivo | O que é |
|---|---|
| `Clyvo-ELLV.postman_collection.json` | Coleção do Postman com os 11 casos e os scripts de verificação |
| `ClyvoELLV-TestesautomatizadosdaAPIpostman_test_run.json` | Resultado exportado da execução no Collection Runner |

### Dados controlados

Os testes usam valores fixos, definidos nas variáveis da coleção:

| Variável | Valor |
|---|---|
| `base_url` | `https://api-clyvo-node.onrender.com` |
| `email` | `dev@clyvovet.dev` (usuário de teste) |
| `senha` | `dev123456` |
| Paciente usado | **Thor** — Cachorro, Pastor Alemão (dado de demonstração) |

As variáveis `token`, `id_animal`, `id_consulta` e `data_amanha` são preenchidas pelos próprios testes durante a execução. A consulta criada no teste é removida no final, então o banco volta ao estado inicial.

### Casos de teste

| Caso | Objetivo | Entrada | Saída esperada |
|---|---|---|---|
| **CT01** — API e banco no ar | API publicada e banco respondendo | `GET /health` | `200`, `status: "ok"`, `banco: true` |
| **CT02** — Login com senha errada | Recusar credenciais inválidas | e-mail `dev@clyvovet.dev`, senha `senhaErrada1` | `401`, "E-mail ou senha incorretos.", sem token |
| **CT03** — Cadastro com senha fraca | Aplicar a política de senha forte | nome `Teste QA`, e-mail `qa.senhafraca@clyvo.dev`, senha `123` | `400`, campo `senha`, regra "mínimo 8 caracteres…" |
| **CT04** — Login válido | Entrar com o usuário de teste | e-mail `dev@clyvovet.dev`, senha `dev123456` | `200`, token JWT, usuário com o mesmo e-mail |
| **CT05** — Rota protegida sem token | Dados só com login | `GET /consultas/agendadas` sem token | `401` com mensagem de sessão |
| **CT06** — Buscar paciente | Busca de pacientes pelo nome | `GET /pacientes?busca=Thor` | `200`, Thor / Cachorro / Pastor Alemão |
| **CT07** — Criar consulta | Agendar consulta para paciente cadastrado | motivo `Teste automatizado - vacina`, data = amanhã, `10:00`, paciente Thor | `201`, status `Agendado`, 2 lembretes (antes e na hora) |
| **CT08** — Consulta nas agendadas | A consulta criada aparece na lista | `GET /consultas/agendadas?busca=Thor` | `200`, consulta do CT07 na lista |
| **CT09** — Concluir consulta | Marcar como realizada com diagnóstico | status `Concluido`, diagnóstico `Cinomose` | `200`, status `Concluido`, diagnóstico salvo, sem lembretes |
| **CT10** — Remover consulta | Remover a consulta do teste | `DELETE /consultas/{id}` | `200`, "Consulta removida…" |
| **CT11** — Consulta removida | Confirmar que a remoção foi real | `GET /consultas/{id}` | `404`, "Consulta não encontrada." |

### Como executar

1. Abra https://api-clyvo-node.onrender.com/health e espere `"banco": true` (a API fica no plano gratuito e pode estar acordando).
2. No **Postman**, clique em **Import** e escolha `Clyvo-ELLV.postman_collection.json`.
3. Na coleção, clique em **Run** → **Run manually** → mantenha os 11 casos na ordem → **Run**.
4. Os testes rodam em sequência: o login salva o token e a criação salva o id da consulta para os passos seguintes.

### Resultado

| Casos | Verificações | Passaram | Falharam | Duração |
|---|---|---|---|---|
| 11 | 29 | **29** | **0** | ~5,4 s |

O resultado completo está em `ClyvoELLV-TestesautomatizadosdaAPIpostman_test_run.json`.

---

## 👥 Integrantes

| RM | Nome | Turma |
|---|---|---|
| RM561713 | Eduardo Batista Locaspi | 2TDSPI |
| RM565799 | Leticia Santiago e Silva | 2TDSPW |
| RM565698 | Liana Lyumi Morisita Fujisima | 2TDSPI |
| RM561833 | Victor Alves Lopes | 2TDSPI |
