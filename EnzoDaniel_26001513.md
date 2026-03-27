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

---

# 4. Requisitos Não Funcionais
- **RNF01:** O sistema deve apresentar tempo de resposta inferior a 2 segundos para consultas de produtos e validações de estoque no balcão.  
- **RNF02:** O sistema deve implementar controle de acesso baseado em papéis (RBAC), distinguindo permissões para Atendente, Farmacêutico, Gerente e Financeiro.  
- **RNF03:** A interface de frente de caixa (PDV) deve ser otimizada para navegação via teclado, garantindo agilidade.  
- **RNF04:** Os dados sensíveis de clientes e receituários médicos devem ser armazenados de forma criptografada no banco de dados.

---

# 5. Casos de Uso

## 5.1. Diagrama Geral
Abaixo está o diagrama de casos de uso geral da aplicação, contemplando os 10 casos mapeados com suas respectivas relações de inclusão e extensão.

<img width="696" height="587" alt="image" src="https://github.com/user-attachments/assets/439d3919-d9a1-480b-8347-5b5d62c249d1" />

---

## 5.2. Documentação por Ator
Nesta seção, os casos de uso e seus diagramas de atividade estão agrupados pelo ator principal, facilitando a compreensão dos processos.

### 5.2.1. Ator: Atendente
Responsável pelo fluxo de frente de caixa (PDV), atendimento direto ao cliente e consultas rápidas.

<img width="696" height="356" alt="image" src="https://github.com/user-attachments/assets/f3d02add-799f-4dbf-9a29-db5a5b0c89bf" />

#### **UC01 — Realizar Venda**
**Ator(es):** Atendente  
**Descrição:** Processo onde o atendente registra os produtos desejados pelo cliente, aplica as regras de negócio e finaliza a compra.  
**Pré-condições:** Usuário logado no sistema com perfil adequado e caixa aberto.  
**Pós-condições:** Venda registrada, estoque atualizado e comprovante emitido.  

**Fluxo Principal**
1. O Atendente inicia uma nova venda no sistema.
2. O Atendente identifica o cliente (opcionalmente via UC03).
3. O Atendente busca e adiciona os produtos desejados.
4. O sistema verifica a disponibilidade no estoque de cada item (Include UC04).
5. O Atendente seleciona a forma de pagamento (À vista).
6. O sistema processa a venda, baixa o estoque e emite o comprovante (Include UC10).

**Fluxos Alternativos / Exceções**
- FA01 — Produto controlado: Se o item adicionado for de uso controlado, o sistema exige autorização (Extend UC06).
- FA02 — Pagamento a prazo: O cliente opta por faturar a compra (Extend UC05).

**Relacionamentos**
- **Include:** UC04, UC10
- **Extend:** UC05, UC06

---

#### **UC02 — Consultar Produto**
**Ator(es):** Atendente (também Farmacêutico e Gerente)  
**Descrição:** Permite buscar os dados básicos e o preço de um produto no banco de dados da farmácia.  
**Pré-condições:** Sistema online.  
**Pós-condições:** Detalhes do produto exibidos na interface.  

**Fluxo Principal**
1. O usuário insere o nome, código de barras ou fabricante na barra de busca.
2. O sistema realiza a pesquisa no banco de dados e retorna as informações.

**Fluxos Alternativos / Exceções**
- FA01 — Produto inexistente: O sistema exibe a mensagem "Produto não localizado".

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** Nenhum

---

#### **UC03 — Identificar Cliente**
**Ator(es):** Atendente  
**Descrição:** Buscar o cadastro de um cliente para vinculá-lo a uma operação.  
**Pré-condições:** Nenhuma.  
**Pós-condições:** Cliente vinculado à operação em andamento.  

**Fluxo Principal**
1. O Atendente solicita o CPF do cliente.
2. O sistema consulta a base de clientes e exibe os dados.

**Fluxos Alternativos / Exceções**
- FA01 — Cliente não cadastrado: O cliente não é encontrado (Extend UC07).
- FA02 — Venda anônima: O cliente se recusa a informar o CPF.

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** UC07

---

#### **UC05 — Registrar Venda a Prazo**
**Ator(es):** Atendente  
**Descrição:** Faturar o valor para o cliente, gerando uma pendência financeira.  
**Pré-condições:** Cliente identificado e com limite de crédito aprovado.  
**Pós-condições:** Lançamento financeiro gerado em "Contas a Receber".  

**Fluxo Principal**
1. O Atendente seleciona a modalidade "A Prazo" durante a venda.
2. O sistema verifica o limite de crédito disponível e autoriza.
3. O sistema cria um título no "Contas a Receber".

**Fluxos Alternativos / Exceções**
- FA01 — Limite excedido: Transação negada, exige outra forma de pagamento.

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** UC01

---

#### **UC07 — Cadastrar Novo Cliente**
**Ator(es):** Atendente  
**Descrição:** Inserção rápida dos dados essenciais de um novo cliente.  
**Pré-condições:** Nenhuma.  
**Pós-condições:** Cliente salvo e apto para operações.  

**Fluxo Principal**
1. O Atendente seleciona a opção de novo cadastro e preenche os dados.
2. O sistema valida os dados e persiste as informações no banco.

**Fluxos Alternativos / Exceções**
- FA01 — CPF duplicado: O sistema bloqueia o cadastro e sugere a busca.

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** UC03


---

### 5.2.2. Ator: Farmacêutico
Responsável pelas validações técnicas e liberação de medicamentos restritos (Tarja Preta/Vermelha).

<img width="696" height="225" alt="image" src="https://github.com/user-attachments/assets/5fb8e5d1-c6d0-4ed5-9822-14140545d6f3" />

#### **UC06 — Autorizar Venda Controlada**
**Ator(es):** Farmacêutico  
**Descrição:** Validação de receituário médico para permitir a liberação de medicamentos.  
**Pré-condições:** Medicamento controlado adicionado à venda.  
**Pós-condições:** Venda liberada e log de autorização registrado.  

**Fluxo Principal**
1. O sistema suspende a inserção do item e exibe a tela de autorização.
2. O Farmacêutico insere credenciais, valida a receita e digita o CRM.
3. O sistema registra a autorização e libera o item.

**Fluxos Alternativos / Exceções**
- FA01 — Autenticação falha: Venda do item permanece bloqueada.

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** UC01

---

### 5.2.3. Ator: Gerente
Responsável pela retaguarda corporativa, controle de estoque e suprimentos.

<img width="687" height="191" alt="image" src="https://github.com/user-attachments/assets/157d6448-bed6-47b4-afc1-6e61a0922ea5" />

#### **UC08 — Registrar Compra**
**Ator(es):** Gerente  
**Descrição:** Entrada de mercadorias no estoque físico a partir de uma nota fiscal.  
**Pré-condições:** Perfil de Gerente logado.  
**Pós-condições:** Estoque atualizado e obrigações financeiras geradas.  

**Fluxo Principal**
1. O Gerente seleciona o Fornecedor e insere os dados da NF.
2. O Gerente inclui os produtos e as quantidades recebidas.
3. O sistema soma as quantidades ao estoque atual (Include UC09).
4. A operação de compra é salva.

**Fluxos Alternativos / Exceções**
- FA01 — Produto não cadastrado: Gerente deve cadastrar o produto base antes.

**Relacionamentos**
- **Include:** UC09
- **Extend:** Nenhum

---

### 5.2.4. Ator: Sistema (Automações)
Representa as rotinas de backend executadas automaticamente sem intervenção humana direta.

<img width="363" height="277" alt="image" src="https://github.com/user-attachments/assets/a3a452d4-a6fa-45aa-b2ad-a99373af7a3d" />

#### **UC04 — Verificar Estoque**
**Ator(es):** Sistema  
**Descrição:** Validação sistêmica da quantidade física disponível.  
**Pré-condições:** Produto selecionado com quantidade desejada.  
**Pós-condições:** Liberação para continuar se saldo for positivo.  

**Fluxo Principal**
1. O sistema recebe a requisição com ID e quantidade.
2. O sistema consulta o saldo e valida se atende à requisição.

**Fluxos Alternativos / Exceções**
- FA01 — Estoque insuficiente: O sistema bloqueia a adição do item.

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** Nenhum

---

#### **UC09 — Gerar Contas a Pagar**
**Ator(es):** Sistema  
**Descrição:** Criação automática de obrigações financeiras (títulos a pagar).  
**Pré-condições:** Compra válida registrada.  
**Pós-condições:** Título financeiro disponível no módulo Pagar.  

**Fluxo Principal**
1. O sistema extrai o valor total e as datas de vencimento da compra.
2. O sistema cria um título atrelado ao Fornecedor com status "Aberta".

**Fluxos Alternativos / Exceções**
- N/A

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** Nenhum

---

#### **UC10 — Emitir Comprovante**
**Ator(es):** Sistema  
**Descrição:** Geração e impressão do recibo da transação.  
**Pré-condições:** Operação principal finalizada.  
**Pós-condições:** Recibo impresso.  

**Fluxo Principal**
1. O sistema consolida dados da venda e estrutura o layout.
2. O sistema despacha o comando de impressão.

**Fluxos Alternativos / Exceções**
- FA01 — Impressora offline: Exibe espelho em tela e habilita envio por e-mail.

**Relacionamentos**
- **Include:** Nenhum
- **Extend:** Nenhum
