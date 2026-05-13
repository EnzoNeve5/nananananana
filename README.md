# 📄 PRD - Sistema de Gestão de Chamados Técnicos

## 1. Visão Geral do Produto
O sistema visa substituir o controle manual (planilhas/papel) de uma rede de supermercados por uma aplicação Java estruturada. O foco é a rastreabilidade de falhas em equipamentos críticos (PDVs, Balanças, Redes).

---

## 2. Objetivos Principais
- **Centralização:** Um único local para todos os chamados.
- **Rastreabilidade:** Saber quem abriu, quem atendeu e quando mudou o status.
- **Segurança:** Acesso apenas via login.
- **Integridade:** Histórico que não pode ser apagado ou editado.

---

## 3. Personas & Perfis
| Perfil | Descrição | Permissões |
| :--- | :--- | :--- |
| **Operador** | Funcionário de TI ou Manutenção | Login, Abrir chamados, Alterar Status, Listar, Filtrar, Ver Histórico. |

---

## 4. Requisitos Funcionais (RF) & Não Funcionais (RNF)

### Requisitos Funcionais
- **RF01:** Login de usuário.
- **RF02:** Cadastro de categoria (Impressoras, Redes, etc).
- **RF03:** Registrar chamado com ID único.
- **RF04:** Data/hora automáticas na abertura.
- **RF05:** Registro automático de Histórico em cada alteração.
- **RF06:** Filtro de listagem por Status.

### Requisitos Não Funcionais
- **RNF01:** Desenvolvimento em **Java**.
- **RNF02:** Persistência em arquivo local (**JSON/CSV**).
- **RNF03:** Interface Desktop simples.
- **RNF04:** Funcionamento 100% offline.

---

## 5. Regras de Negócio (RN)
1. **RN01:** Descrição é campo obrigatório.
2. **RN02:** O status inicial é sempre **Aberto**.
3. **RN03:** Só é possível finalizar (Concluído) se o chamado estiver **Em Atendimento**.
4. **RN04:** Chamados com status **Concluído** tornam-se somente leitura.
5. **RN05:** Nenhuma entrada de histórico pode ser deletada (Auditoria).

---

## 6. Fluxo de Trabalho (Workflow)
1. **Login:** Autenticação obrigatória.
2. **Abertura:** Registro do problema + Categoria + Prioridade.
3. **Transição:** Aberto -> Em Atendimento -> [Aguardando Peça/Usuário/Obs] -> Concluído.
4. **Encerramento:** Bloqueio de edição e registro final.

---

## 7. Modelo de Dados (Entidades)
- **Usuario:** `id, nome, login, senha`
- **Categoria:** `id, nome, descricao`
- **Chamado:** `id, descricao, dataAbertura, statusAtual, prioridade, categoria, usuario`
- **Historico:** `id, idChamado, timestamp, usuario, statusAnterior, statusNovo, observacao`

---

## 8. Cenários de Teste (UAT)
- **Cenário A:** Tentar finalizar um chamado "Aberto". *Resultado esperado:* Erro (RN03).
- **Cenário B:** Alterar status de "Aberto" para "Em Atendimento". *Resultado esperado:* Novo registro na tabela de Histórico.
- **Cenário C:** Tentar editar um chamado "Concluído". *Resultado esperado:* Acesso negado (RN04).
