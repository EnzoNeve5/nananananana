# Product Requirements Document (PRD) - Sistema de Gestão de Chamados Técnicos

**Projeto:** Sistema de Chamados (Help Desk Interno)  
**Versão:** 1.0  
**Status:** Em Definição  
**Data:** 13/05/2026  

---

## 1. Visão Geral
### 1.1 Resumo do Projeto
Este projeto visa substituir o controle manual e via planilhas de problemas técnicos em uma rede de supermercados por um sistema centralizado, seguro e estruturado em Java. O foco é garantir rastreabilidade, histórico e eficiência no suporte de TI para as cinco lojas da rede.

### 1.2 Objetivos Principais
* Centralizar o registro de falhas em hardware (computadores, impressoras), rede e sistemas.
* Implementar controle de acesso por login para auditoria.
* Automatizar a geração de histórico e controle de status.
* Garantir que o suporte técnico siga regras de negócio rígidas para finalização de chamados.

---

## 2. Contexto e Problema
### 2.1 Situação Atual
Atualmente, os registros são feitos em papel ou planilhas compartilhadas.
* **Falta de Segurança:** Sem controle de quem alterou os dados.
* **Falta de Histórico:** Não há visibilidade de quando um status mudou ou quem foi o responsável.
* **Retrabalho:** Problemas recorrentes são tratados como novos por falta de base de conhecimento.

### 2.2 Consequências
* Perda de informações críticas.
* Dificuldade extrema em priorizar problemas graves.
* Impossibilidade de realizar auditoria em caso de falhas de segurança ou processos.

---

## 3. Perfis de Usuário
### 3.1 Operador (TI / Suporte Interno)
O único perfil previsto para esta versão do sistema.
* **Ações:** Login, cadastro de categorias, abertura de chamados, alteração de status, registro de notas técnicas e visualização de históricos.

---

## 4. Requisitos do Produto

### 4.1 Requisitos Funcionais (RF)
| ID | Nome | Descrição |
|:---|:---|:---|
| **RF01** | Autenticação | O sistema deve exigir login/senha para qualquer operação. |
| **RF02** | Gestão de Categorias | O usuário deve poder cadastrar categorias (ex: Impressora, Rede). |
| **RF03** | Abertura de Chamado | Registro obrigatório de descrição, categoria e prioridade. |
| **RF04** | Timestamps | O sistema deve registrar automaticamente data/hora de abertura e de cada alteração. |
| **RF05** | Controle de Status | Transições entre: Aberto, Atendimento, Peça, Usuário, Observação, Concluído. |
| **RF06** | Registro de Histórico | Toda mudança de status deve criar uma entrada imutável no histórico. |
| **RF07** | Notas e Observações | Permitir inclusão de textos explicativos durante o atendimento. |
| **RF08** | Listagem e Filtro | Visualizar chamados com filtro por status atual. |

### 4.2 Requisitos Não Funcionais (RNF)
| ID | Categoria | Especificação |
|:---|:---|:---|
| **RNF01** | Tecnologia | Desenvolvido em **Java**. |
| **RNF02** | Armazenamento | Persistência local via arquivos **JSON ou CSV** (sem necessidade de DB externo). |
| **RNF03** | Disponibilidade | Funcionamento **Offline** (ambiente de rede instável). |
| **RNF04** | Escalabilidade | Suporte para até 500 registros de chamados. |

---

## 5. Regras de Negócio (RN)
* **RN01:** A descrição do chamado é campo obrigatório.
* **RN02:** O status inicial de qualquer chamado é sempre "Aberto".
* **RN03:** Um chamado só pode ser finalizado (Concluído) se o status imediatamente anterior for "Em Atendimento".
* **RN04:** Uma vez "Concluído", o chamado torna-se leitura apenas (não pode ser reaberto ou editado).
* **RN05:** Registros de histórico não podem ser excluídos ou editados (Audit Log).
* **RN06:** Login é obrigatório para acessar qualquer tela do sistema.

---

## 6. Modelo de Dados (Entidades)
### 6.1 Usuário
* `id`, `nome`, `login`, `senha`
### 6.2 Chamado
* `id` (Auto-incremento), `descricao`, `dataAbertura`, `status`, `prioridade`, `categoriaId`, `usuarioId`
### 6.3 Histórico
* `id`, `chamadoId`, `dataHora`, `usuarioResponsavel`, `statusAnterior`, `statusNovo`, `observacao`

---

## 7. Fluxo de Processo
1. **Início:** Login do Operador.
2. **Ação:** Abertura de Chamado (Status: Aberto).
3. **Atendimento:** Alteração para "Em Atendimento".
4. **Ciclo de Espera:** Pode alternar entre "Aguardando Peça" ou "Aguardando Usuário" (sempre voltando para "Atendimento").
5. **Fim:** Conclusão do chamado (somente a partir de "Em Atendimento").

---
