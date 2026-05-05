# Architecture Decision Record (ADR)

## ADR-001: Technology Stack - Sctech Marketplace

**Data:** 2026-05-05  
**Status:** APROVADO  
**Contexto:** Definição da stack tecnológica para o desenvolvimento do Sctech Marketplace

---

## 1. Decisão

Adotamos a seguinte stack tecnológica para o desenvolvimento do Sctech Marketplace:

### Frontend
- **Framework:** Angular (v17+)
- **Linguagem:** TypeScript
- **Gerenciador de Pacotes:** npm
- **Build Tool:** Angular CLI
- **Estilização:** SCSS/CSS3
- **Gerenciamento de Estado:** RxJS + Angular Services / NgRx (opcional para estado complexo)

### Backend
- **Runtime:** Node.js (v18+)
- **Framework:** Express.js ou NestJS
- **Linguagem:** TypeScript
- **Gerenciador de Pacotes:** npm
- **API:** REST com OpenAPI/Swagger

### Banco de Dados
- **SGBD:** SQLite 3
- **ORM:** Sequelize ou TypeORM
- **Migrations:** Gerenciadas pela ORM

### DevOps e Ferramentas
- **Controle de Versão:** Git + GitHub/GitLab
- **Containerização:** Docker (opcional)
- **Testing (Backend):** Jest + Supertest
- **Testing (Frontend):** Jasmine + Karma / Jest
- **Linting:** ESLint + Prettier
- **CI/CD:** GitHub Actions / GitLab CI (futuro)

---

## 2. Rationale (Justificativa)

### Angular
- Framework robusto e maduro para SPA
- Excelente suporte a TypeScript nativo
- Comunidade grande e documentação abrangente
- Ideal para aplicações complexas com múltiplos módulos

### Node.js + Express/NestJS
- JavaScript/TypeScript em ambos frontend e backend (Full Stack JS)
- Excelente performance e escalabilidade
- Grande ecossistema de pacotes (npm)
- NestJS oferece arquitetura mais estruturada (recomendado para equipes maiores)

### SQLite
- Banco de dados embarcado, sem servidor separado
- Ideal para MVP e prototipagem rápida
- Migração futura para PostgreSQL/MySQL é simples
- Zero configuração necessária
- Suficiente para marketplace com moderado volume de dados

### TypeScript
- Tipagem estática em todo o stack
- Reduz bugs em tempo de execução
- Melhor experiência de desenvolvimento com IDE
- Facilita manutenção e refatoração

---

## 3. Estrutura de Pastas Recomendada

```
sctech-marketplace/
├── docs/
│   ├── ADR.md (este arquivo)
│   ├── API.md
│   └── SETUP.md
├── backend/
│   ├── src/
│   │   ├── modules/
│   │   ├── common/
│   │   ├── config/
│   │   └── main.ts
│   ├── test/
│   ├── package.json
│   └── tsconfig.json
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── modules/
│   │   │   ├── shared/
│   │   │   └── app.component.ts
│   │   ├── assets/
│   │   └── main.ts
│   ├── package.json
│   └── angular.json
├── .gitignore
├── docker-compose.yml (opcional)
└── README.md
```

---

## 4. Alternativas Consideradas

| Tecnologia | Considerada | Motivo da Rejeição |
|------------|-------------|-------------------|
| React | Sim | Angular oferece melhor estrutura enterprise |
| Vue.js | Sim | Comunidade menor em contexto enterprise |
| Django/Flask | Sim | Preferência por Node.js para Full Stack |
| PostgreSQL | Sim | SQLite suficiente para MVP |
| MongoDB | Sim | SQLite melhor para dados estruturados |

---

## 5. Riscos e Mitigações

| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|--------|-----------|
| SQLite não escalar com volume de dados | Baixa | Alto | Planejar migração para PostgreSQL |
| Curva de aprendizado Angular | Média | Médio | Treinamento da equipe e documentação |
| Performance em produção | Baixa | Alto | Load testing e otimizações de cache |

---

## 6. Próximos Passos

1. ✅ Configuração inicial do repositório Git
2. ⏳ Setup do projeto Node.js + Express/NestJS
3. ⏳ Setup do projeto Angular
4. ⏳ Definição de Entidades e Schema do Banco de Dados
5. ⏳ Implementação de autenticação e autorização
6. ⏳ Criação de CI/CD pipeline
7. ⏳ Deployment em ambiente de staging

---

## 7. Referências

- [Angular Documentation](https://angular.io/docs)
- [NestJS Documentation](https://docs.nestjs.com/)
- [Node.js Best Practices](https://nodejs.org/en/docs/)
- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [TypeORM Documentation](https://typeorm.io/)

---

**Aprovado por:** Product Owner - Sctech  
**Data de Revisão:** 2026-05-05  
**Próxima Revisão:** Conforme necessidade
