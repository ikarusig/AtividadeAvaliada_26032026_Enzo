# Avaliação — Engenharia de Software
**Sistema Integrado de Gestão de Farmácia — MVP Definido pelo Estudante**

**Aluno:** *`Enzo Daniel Abreu`* || **RA:** *`26001513`* || Data: *`26/03/2026`*

---

# 1. Definição do MVP
O MVP (Minimum Viable Product) focado nesta entrega abrange o **Core Business Operacional** da farmácia: o fluxo de vendas no balcão e a gestão primária de estoque e compras. 

- **O que está dentro do MVP:** O processo de venda (identificação de cliente, consulta de produto e estoque, autorização de controlados e finalização à vista/prazo), além do registro de compras de fornecedores integrando automaticamente com o contas a pagar e atualização de estoque.
- **O que está fora do MVP:** Relatórios gerenciais complexos, gestão de recursos humanos, painéis de auditoria avançada e controle de perdas/transferências entre diferentes unidades.
- **Por que fiz essas escolhas:** Garantir o funcionamento impecável da frente de caixa e a consistência imediata do estoque e do financeiro básico é o que traz o maior valor inicial para a rede "Saúde & Vida", resolvendo seus problemas críticos de divergência e lentidão no atendimento.

---

# 2. Regras de Negócio
- **RN01:** Produtos sem saldo disponível em estoque não podem ter sua venda registrada ou concluída.
- **RN02:** A venda de medicamentos controlados exige obrigatoriamente a autenticação e autorização de um usuário com perfil de Farmacêutico, bem como o registro da receita.
- **RN03:** Toda venda a prazo concluída com sucesso deve gerar automaticamente um lançamento com status "Aberta" no módulo de Contas a Receber.
- **RN04:** O saldo de estoque da unidade deve ser atualizado (reduzido ou incrementado) em tempo real após a confirmação de uma venda ou entrada de compra de fornecedor.
- **RN05:** O sistema deve bloquear lançamentos financeiros (pagar/receber) com datas de vencimento retroativas à data atual do sistema.

---

# 3. Requisitos Funcionais
- **RF01:** O sistema deve permitir a consulta de produtos por nome, código de barras ou fabricante.
- **RF02:** O sistema deve permitir a identificação de clientes via CPF no momento do atendimento.
- **RF03:** O sistema deve permitir o cadastro rápido de novos clientes diretamente no balcão.
- **RF04:** O sistema deve verificar a disponibilidade de estoque antes de adicionar um item à venda.
- **RF05:** O sistema deve permitir a finalização de vendas nas modalidades "à vista" ou "a prazo".
- **RF06:** O sistema deve emitir um comprovante detalhado ao final de cada transação de venda.
- **RF07:** O sistema deve permitir o registro de compras de mercadorias vinculando a um fornecedor.
- **RF08:** O sistema deve gerar contas a pagar automaticamente após o registro de uma compra de fornecedor.
