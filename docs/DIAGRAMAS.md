# Diagramas - Sctech Marketplace

## 1. Fluxo de Busca e Adição ao Carrinho

Diagrama que mostra o percurso completo do usuário desde o acesso à aplicação até adicionar um produto ao carrinho.

### Fluxograma do Usuário - Busca e Carrinho

```mermaid
flowchart TD
    Start([Usuário Acessa App]) --> HomePage[Página Inicial]
    HomePage --> SearchInput{Usuário Deseja<br/>Buscar Produto?}
    
    SearchInput -->|Sim| SearchPage[Vai para Página de Busca]
    SearchInput -->|Não| BrowseCategories[Navega por Categorias]
    
    SearchPage --> EnterSearch[Digita Termo de Busca]
    EnterSearch --> ClickSearch[Clica em Buscar]
    ClickSearch --> LoadResults[Sistema Carrega Resultados]
    
    BrowseCategories --> ViewProducts[Exibe Produtos da Categoria]
    LoadResults --> ViewProducts
    
    ViewProducts --> ProductFound{Encontrou<br/>Produto Desejado?}
    
    ProductFound -->|Não| SearchAgain[Tenta Nova Busca]
    SearchAgain --> EnterSearch
    
    ProductFound -->|Sim| ViewDetails[Clica no Produto]
    ViewDetails --> ProductPage[Exibe Detalhes do Produto]
    ProductPage --> CheckStock{Produto em<br/>Estoque?}
    
    CheckStock -->|Não| OutOfStock[Exibe Mensagem: Indisponível]
    OutOfStock --> SearchAgain
    
    CheckStock -->|Sim| SelectQuantity[Seleciona Quantidade]
    SelectQuantity --> AddToCart[Clica em Adicionar ao Carrinho]
    AddToCart --> ValidateQty{Quantidade<br/>Válida?}
    
    ValidateQty -->|Não| QtyError[Exibe Erro de Quantidade]
    QtyError --> SelectQuantity
    
    ValidateQty -->|Sim| AddSuccess[Produto Adicionado ao Carrinho]
    AddSuccess --> ShowNotification[Exibe Notificação de Sucesso]
    ShowNotification --> ContinueShopping{Continuar<br/>Comprando?}
    
    ContinueShopping -->|Sim| HomePage
    ContinueShopping -->|Não| ViewCart[Vai para Carrinho]
    ViewCart --> End([Finaliza Fluxo])
    
    style Start fill:#90EE90
    style End fill:#FFB6C6
    style AddSuccess fill:#87CEEB
    style OutOfStock fill:#FFB347
    style QtyError fill:#FFB347
```

### Explicação do Fluxo

#### **Fase 1: Descoberta de Produtos**
1. Usuário acessa a aplicação na página inicial
2. Escolhe entre:
   - **Buscar**: Ir para página de busca e digitar termo
   - **Navegar**: Explorar produtos por categorias

#### **Fase 2: Busca e Resultados**
3. Se buscou, o sistema carrega resultados baseado na query
4. Se navegou, exibe produtos da categoria selecionada
5. Sistema valida se encontrou correspondências

#### **Fase 3: Visualização do Produto**
6. Usuário clica no produto desejado
7. Sistema exibe página de detalhes com:
   - Descrição completa
   - Imagens
   - Preço
   - Disponibilidade em estoque

#### **Fase 4: Validações**
8. Sistema verifica se o produto está em estoque
9. Se indisponível: Exibe mensagem e retorna à busca
10. Se disponível: Permite seleção de quantidade

#### **Fase 5: Adição ao Carrinho**
11. Usuário seleciona a quantidade desejada
12. Clica em "Adicionar ao Carrinho"
13. Sistema valida quantidade (mínimo, máximo, estoque)
14. Se válida: Adiciona e exibe confirmação
15. Se inválida: Exibe erro e retorna à seleção

#### **Fase 6: Próximo Passo**
16. Usuário decide se continua comprando ou vai ao carrinho
17. Fluxo encerra na visualização do carrinho

### Pontos de Validação

| Validação | Tipo | Ação em Falha |
|-----------|------|---------------|
| Estoque disponível | Sistema | Exibe indisponível |
| Quantidade válida | Sistema | Mostra erro |
| Termo de busca | Cliente | Campo obrigatório |
| Seleção de quantidade | Cliente | Deve ser número positivo |

### Estados da Aplicação

```
┌─────────────────┐
│ HOME PAGE       │ ◄──────────────────────┐
└────────┬────────┘                        │
         │                                 │
    ┌────┴─────────────────────┐          │
    │                          │          │
    ▼                          ▼          │
┌───────────┐          ┌──────────────┐  │
│ SEARCH    │          │ BROWSE       │  │
└─────┬─────┘          └──────┬───────┘  │
      │                       │          │
      └───────────┬───────────┘          │
                  │                      │
                  ▼                      │
         ┌─────────────────┐             │
         │ PRODUCT DETAILS │             │
         └────────┬────────┘             │
                  │                      │
           ┌──────┴──────┐               │
           │             │               │
          (Em Estoque)  (Sem Estoque)    │
           │             │               │
           ▼             └───────────────┘
    ┌──────────────┐
    │ SELECT QUANTITY │
    └─────┬────────┘
          │
          ▼
    ┌──────────────┐
    │ CART         │
    └──────┬───────┘
           │
      ┌────┴────────────┐
      │                 │
   (Continue)      (Checkout)
      │                 │
      └─────────────────┘
```

---

## 2. Diagrama de Entidades e Relacionamentos (ER)

Diagrama que apresenta a estrutura do banco de dados com todas as entidades e seus relacionamentos.

### Diagrama ER do Marketplace

```mermaid
erDiagram
    USER ||--o{ CART : owns
    USER ||--o{ ORDER : places
    PRODUCT ||--o{ CART_ITEM : contains
    CART ||--|{ CART_ITEM : has
    PRODUCT ||--o{ ORDER_ITEM : contains
    ORDER ||--|{ ORDER_ITEM : has
    
    USER {
        int userId PK
        string email UK
        string name
        string phone
        text address
        datetime createdAt
    }
    
    PRODUCT {
        int productId PK
        string name
        text description
        decimal price
        int stock
        string category
        datetime createdAt
    }
    
    CART {
        int cartId PK
        int userId FK
        decimal totalPrice
        datetime createdAt
        datetime updatedAt
    }
    
    CART_ITEM {
        int cartItemId PK
        int cartId FK
        int productId FK
        int quantity
        decimal priceAtTime
    }
    
    ORDER {
        int orderId PK
        int userId FK
        string status
        decimal totalAmount
        text shippingAddress
        datetime createdAt
        datetime completedAt
    }
    
    ORDER_ITEM {
        int orderItemId PK
        int orderId FK
        int productId FK
        int quantity
        decimal pricePerUnit
        decimal subtotal
    }
```

### Explicação das Entidades

#### USER (Usuário)
- Armazena dados dos clientes do marketplace
- **PK**: userId (chave primária)
- **UK**: email (chave única)
- Atributos: nome, telefone, endereço, data de criação

#### PRODUCT (Produto)
- Catálogo de produtos disponíveis
- Controla estoque e preço
- Categorizado e com descrição detalhada

#### CART (Carrinho)
- Armazena o carrinho temporário do usuário
- Relacionado com um **USER**
- Pode conter múltiplos **CART_ITEM**
- Mantém preço total atualizado

#### CART_ITEM (Item do Carrinho)
- Liga **PRODUCT** ao **CART**
- Armazena quantidade e preço no momento da adição
- Permite rastreamento de histórico de preços

#### ORDER (Pedido)
- Representa um pedido realizado pelo usuário
- Mantém histórico completo de compras
- Status pode ser: pendente, processando, enviado, entregue, cancelado
- Armazena endereço de entrega específico

#### ORDER_ITEM (Item do Pedido)
- Liga **PRODUCT** ao **ORDER**
- Mantém registro histórico do preço praticado na época da compra
- Calcula subtotal por item

### Relacionamentos

```
USER (1) ──owns──────────────o{ (N) CART
USER (1) ──places────────────o{ (N) ORDER
PRODUCT (1) ──contains──────o{ (N) CART_ITEM
CART (1) ──has─────────────{ (N) CART_ITEM
PRODUCT (1) ──contains──────o{ (N) ORDER_ITEM
ORDER (1) ──has────────────{ (N) ORDER_ITEM
```

### Cardinalidade

- **1 USER** possui **1 CART** ativo
- **1 USER** pode fazer **N ORDERs**
- **1 CART** pode ter **N CART_ITEMs**
- **1 ORDER** pode ter **N ORDER_ITEMs**
- **1 PRODUCT** pode estar em múltiplos **CART_ITEMs** e **ORDER_ITEMs**

---

## Notas Técnicas

### Frontend (Angular)
- Página de busca com autocomplete
- Paginação de resultados
- Filtros por categoria, preço, avaliação
- Validação de quantidade com min/max
- Toast/Snackbar para notificações

### Backend (Node.js/Express/NestJS)
- Endpoint GET `/api/products/search?query=...`
- Endpoint GET `/api/products/:id`
- Endpoint POST `/api/cart/add`
- Validação de estoque antes de confirmar adição
- Transação para garantir consistência

### Banco de Dados (SQLite)
- Tabela `products` com coluna `stock`
- Tabela `cart_items` com `product_id`, `quantity`
- Índices em `name` e `category` para busca rápida

---

## Próximos Diagramas

- [ ] Fluxo de Checkout e Pagamento
- [ ] Fluxo de Autenticação (Login/Registro)
- [ ] Fluxo de Gerenciamento de Pedidos
- [ ] Arquitetura de Sistema
- [ ] Sequência de Requisições API
