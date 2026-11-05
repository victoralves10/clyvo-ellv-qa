<div align="center">

# 🐾 Clyvo ELLV — Testes 🧪

**Compliance, Quality Assurance & Tests · Sprint 4**
Challenge FIAP 2026 em parceria com a **Clyvo Vet**

[![Vídeo](https://img.shields.io/badge/▶_Vídeo_da_automação-YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://youtu.be/t1ihDwkuGZU)
[![Azure Boards](https://img.shields.io/badge/Plano_de_testes-Azure_Boards-0078D7?style=for-the-badge&logo=azuredevops&logoColor=white)](https://dev.azure.com/RM561713/ELLV-Solu%C3%A7%C3%A3o-Mobile/_backlogs/backlog/ELLV-Solu%C3%A7%C3%A3o-Mobile%20Team/Epics)
[![API](https://img.shields.io/badge/API-Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)](https://api-clyvo-node.onrender.com/docs)

</div>

---

O **Clyvo ELLV** é um aplicativo mobile de gestão veterinária: cadastro de pacientes e responsáveis, histórico de consultas, lembretes no celular e alerta de possível surto por região. O app consome uma API REST própria, publicada na nuvem.

---

## 🔗 Links da entrega

| Item | Link |
|---|---|
| **Parte A — Plano de testes manuais (Azure Boards)** | [Abrir no Azure DevOps](https://dev.azure.com/RM561713/ELLV-Solu%C3%A7%C3%A3o-Mobile/_backlogs/backlog/ELLV-Solu%C3%A7%C3%A3o-Mobile%20Team/Epics) |
| **Parte B — Vídeo da automação** | [Assistir no YouTube](https://youtu.be/t1ihDwkuGZU) |
| API testada | [api-clyvo-node.onrender.com](https://api-clyvo-node.onrender.com) |
| Documentação da API (Swagger) | [api-clyvo-node.onrender.com/docs](https://api-clyvo-node.onrender.com/docs) |

---

## 🤖 Parte B — Automação de testes (Postman)

Os testes automatizados validam a **API REST** que sustenta as telas do aplicativo: login, cadastro, pacientes e consultas. Foram feitos **11 casos de teste** (o mínimo pedido é 4), com **29 verificações**.

### Arquivos

| Arquivo | O que é |
|---|---|
| [`Clyvo-ELLV.postman_collection.json`](Clyvo-ELLV.postman_collection.json) | Coleção do Postman com os 11 casos e os scripts de verificação |
| [`ClyvoELLV-TestesautomatizadosdaAPIpostman_test_run.json`](ClyvoELLV-TestesautomatizadosdaAPIpostman_test_run.json) | Resultado exportado da execução no Collection Runner |

### Dados controlados

| Variável | Valor |
|---|---|
| `email` | `dev@clyvovet.dev` (usuário de teste) |
| `senha` | `dev123456` |
| Paciente usado | **Thor** — Cachorro, Pastor Alemão |

### Casos de teste

| Caso | Objetivo | Entrada | Saída esperada | |
|---|---|---|---|:-:|
| **CT01** — API e banco no ar | API publicada e banco respondendo | `GET /health` | `200`, banco conectado | ✅ |
| **CT02** — Login com senha errada | Recusar credenciais inválidas | senha `senhaErrada1` | `401`, "E-mail ou senha incorretos." | ✅ |
| **CT03** — Cadastro com senha fraca | Aplicar a política de senha forte | senha `123` | `400`, regra "mínimo 8 caracteres…" | ✅ |
| **CT04** — Login válido | Entrar com o usuário de teste | `dev@clyvovet.dev` / `dev123456` | `200`, devolve o token | ✅ |
| **CT05** — Rota protegida sem token | Dados só com login | `GET /consultas/agendadas` sem token | `401` | ✅ |
| **CT06** — Buscar paciente | Busca pelo nome | `busca=Thor` | `200`, Thor / Cachorro / Pastor Alemão | ✅ |
| **CT07** — Criar consulta | Agendar consulta | paciente Thor, amanhã às `10:00` | `201`, status `Agendado`, 2 lembretes | ✅ |
| **CT08** — Consulta nas agendadas | A consulta aparece na lista | `busca=Thor` | `200`, consulta do CT07 na lista | ✅ |
| **CT09** — Concluir consulta | Marcar como realizada | status `Concluido`, diagnóstico `Cinomose` | `200`, diagnóstico salvo | ✅ |
| **CT10** — Remover consulta | Remover a consulta do teste | `DELETE /consultas/{id}` | `200`, "Consulta removida…" | ✅ |
| **CT11** — Consulta removida | Confirmar a remoção | `GET /consultas/{id}` | `404`, "Consulta não encontrada." | ✅ |

### Resultado

| Casos | Verificações | ✅ Passaram | ❌ Falharam | ⏱️ Duração |
|:-:|:-:|:-:|:-:|:-:|
| **11** | **29** | **29** | **0** | ~5,4 s |

---

## 👥 Integrantes

| RM | Nome | Turma |
|---|---|---|
| RM561713 | Eduardo Batista Locaspi | 2TDSPI |
| RM565799 | Leticia Santiago e Silva | 2TDSPW |
| RM565698 | Liana Lyumi Morisita Fujisima | 2TDSPI |
| RM561833 | Victor Alves Lopes | 2TDSPI |
