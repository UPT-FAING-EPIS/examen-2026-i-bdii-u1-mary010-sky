# RecipeSystem 🍽️

Sistema de gestión de recetas y menús semanales personalizados.

## Stack
- **Backend:** .NET 8, Clean Architecture, MediatR, EF Core, PostgreSQL
- **Frontend:** Angular 18+, Standalone Components, Signals
- **Infra:** AWS (ECS Fargate + RDS + S3 + CloudFront), Terraform, GitHub Actions

## Inicio rápido

### Con Docker Compose
```bash
docker compose up --build
# API: http://localhost:8080/swagger
# App: http://localhost:4200
```

### Manual

**Backend:**
```bash
# 1. Levantar PostgreSQL
docker run -d --name recipe-postgres -e POSTGRES_DB=recipe_db -e POSTGRES_USER=postgres -e POSTGRES_PASSWORD=postgres -p 5432:5432 postgres:16

# 2. Migraciones
dotnet tool install --global dotnet-ef
dotnet ef database update --project src/RecipeSystem.Infrastructure --startup-project src/RecipeSystem.API

# 3. Ejecutar API
cd src/RecipeSystem.API && dotnet run
# Swagger: https://localhost:7001/swagger
```

**Frontend:**
```bash
cd frontend
npm install
npm start
# App: http://localhost:4200
```

## Estructura
```
RecipeSystem/
├── src/
│   ├── RecipeSystem.API/          ← Controllers, Program.cs
│   ├── RecipeSystem.Application/  ← Casos de uso (CQRS)
│   ├── RecipeSystem.Domain/       ← Entidades y lógica de negocio
│   └── RecipeSystem.Infrastructure/ ← EF Core, repositorios
├── frontend/                      ← Angular 18+
├── infra/                         ← Terraform (AWS)
└── .github/workflows/             ← CI/CD pipelines
```

## Algoritmo de menú
El `MenuGeneratorService` genera menús 7 días × 4 comidas:
1. Filtra recetas según alergias y condiciones médicas del usuario
2. Puntúa recetas según el objetivo (bajar peso, ganar músculo, etc.)
3. Selección ponderada con algo de aleatoriedad para variedad
