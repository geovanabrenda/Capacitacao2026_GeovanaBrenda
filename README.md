Alterações por Geovana Brenda
# ✅ **Sugestões de Evolução — Regras de Negócio e Melhorias (para os alunos)**

Este projeto foi criado como introdução ao Java e pode ser ampliado com novas regras de negócio, entidades, validações e funcionalidades.
As sugestões abaixo servem como **exercícios guiados** para aprimorar o domínio de API REST, Java, Spring Boot, autenticação, modelagem de dados e boas práticas.

As funcionalidades estão organizadas por prioridade e dificuldade.

---

## ✅ 1. Categorias e Organização do Catálogo

**Prioridade:** Alta
**Dificuldade:** Baixa

### Regras:

* Todo produto deve pertencer a uma categoria.
* Categorias podem ter hierarquia (pai → filho).
* Nome de categoria deve ser único no mesmo nível.

### Endpoints sugeridos:

```
GET    /categories
POST   /categories
PUT    /categories/{id}
DELETE /categories/{id}
```

### Validações:

* Nome obrigatório.
* Proibir duplicidade.

---

## ✅ 2. Controle de Estoque (Inventário)

**Prioridade:** Alta
**Dificuldade:** Média

### Regras:

* Cada ajuste de estoque gera um registro de `InventoryTransaction`.
* A venda/pedido deve diminuir o estoque.
* Impedir vendas com estoque insuficiente.
* Notificar quando um produto atingir estoque mínimo (pode ser apenas flag).

### Tipos de transação:

* Entrada (compra/fornecedor)
* Saída (venda)
* Ajuste
* Devolução

### Endpoints sugeridos:

```
POST /inventory/{productId}/add
POST /inventory/{productId}/remove
GET  /inventory/{productId}
```

---

## ✅ 3. Carrinho de Compras

**Prioridade:** Alta
**Dificuldade:** Média

### Regras:

* Usuário autenticado pode ter apenas 1 carrinho ativo.
* Itens têm `priceSnapshot` (preço do momento).
* Atualizações recalculam totais.

### Endpoints sugeridos:

```
GET  /cart
POST /cart/items
PUT  /cart/items/{itemId}
DELETE /cart/items/{itemId}
```

---

## ✅ 4. Pedidos (Orders)

**Prioridade:** Alta
**Dificuldade:** Média

### Regras:

* Carrinho → Pedido (checkout).
* Status do pedido:

    * `CREATED`
    * `PAID`
    * `SHIPPED`
    * `DELIVERED`
    * `CANCELLED`
* Cancelamento permitido somente em `CREATED` ou `PAID`.

### Endpoints sugeridos:

```
POST /orders
GET  /orders/{id}
POST /orders/{id}/cancel
```

---

## ✅ 5. Promoções e Cupons

**Prioridade:** Média
**Dificuldade:** Média

### Tipos:

* Desconto percentual (%)
* Desconto fixo (R$)
* Promoção por categoria ou produto
* Cupom válido por período
* Cupom com limite de uso

### Validações:

* Cupom expirado → rejeitar
* Cupom já utilizado pelo usuário → rejeitar
* Cupom sem relação com produtos do carrinho → rejeitar

### Endpoints:

```
POST /promotions
POST /coupons/apply
```

---

## ✅ 6. Reviews e Avaliações

**Prioridade:** Baixa
**Dificuldade:** Baixa

### Regras:

* Apenas quem comprou pode avaliar.
* Limite de 1 avaliação por produto por pedido.
* Recalcular média a cada novo review.

### Endpoints:

```
POST /reviews
GET  /reviews/product/{productId}
```

---

## ✅ 7. Auditoria (Audit Log)

**Prioridade:** Média
**Dificuldade:** Baixa

### Regras:

* Registrar:

    * quem criou/alterou/deletou
    * data e hora
    * antes e depois da alteração (JSON)
* Auditoria deve ser imutável.

### Endpoints:

```
GET /audit?entity=Product
```

---

## ✅ 8. Relatórios e Métricas

**Prioridade:** Baixa
**Dificuldade:** Média

### Exemplos:

* Produtos mais vendidos.
* Faturamento por período.
* Produtos com estoque baixo.
* Promoções mais utilizadas.

### Endpoints:

```
GET /reports/sales
GET /reports/top-products
GET /reports/low-stock
```

---

# ✅ 9. Novas Entidades Sugeridas

```text
Product
- id
- name
- description
- sku
- price
- costPrice
- categoryId
- stockQuantity
- active
- createdAt
- updatedAt

Category
- id
- name
- parentId
- createdAt
- updatedAt

InventoryTransaction
- id
- productId
- delta
- reason
- referenceId
- createdBy
- createdAt

Cart
- id
- userId
- status

CartItem
- id
- cartId
- productId
- quantity
- priceSnapshot

Order
- id
- userId
- total
- discount
- freight
- status
- createdAt
- address

OrderItem
- id
- orderId
- productId
- quantity
- priceSnapshot

Promotion
- id
- code
- type
- value
- validFrom
- validTo
- usageLimit
- usedCount
- applicableTo

Review
- id
- productId
- userId
- rating
- comment
- createdAt

AuditLog
- id
- entityType
- entityId
- action
- beforeJson
- afterJson
- who
- when
```

---

# ✅ 10. Tarefas / Exercícios Práticos para os Alunos

## 🟦 **Básico (1–2 horas)**

* Criar entidade Categoria.
* Associar Produto → Categoria.
* Implementar busca de produtos por nome/categoria.
* Validar dados básicos (preço > 0, nome obrigatório).

## 🟩 **Intermediário (4–8 horas)**

* Criar carrinho de compras.
* Controlar estoque com `InventoryTransaction`.

## 🟧 **Avançado (8–20 horas)**

* Finalizar fluxo completo de pedidos.
* Criar sistema de cupons e promoções.
* Implementar reviews vinculados ao pedido.
* Criar testes unitários e de integração.

## 🟥 **Desafios bônus**

* Multi-seller (cada vendedor gerencia seus produtos).
* Notificações (e-mail ou webhook) ao mudar status do pedido.
* Agendamento (Scheduler) para alertas de estoque baixo.

---


# Contatos

* fabreum.dev@gmail.com
* fabreum@minsait.com
* linkedin.com/in/fabreum