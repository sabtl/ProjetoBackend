# ProjetoBackend
Alunos: Sabrina Bettiol, Miguel Arcanjo
Tema:  Hábitos e Organização Pessoal: OrganizeMe 

# 📚 Documentação da API

## 🧩 Entidades (Modelos)

### Usuário (`User`)
Representa a pessoa que usa o aplicativo para registrar e acompanhar seus hábitos.

| Campo      | Tipo        | Descrição                     |
|-----------|------------|--------------------------------|
| `id`      | string (UUID) | Identificador único do usuário |
| `name`    | string     | Nome completo do usuário       |
| `email`   | string     | Email do usuário (único)      |
| `password`| string     | Senha criptografada           |
| `createdAt` | Date     | Data de criação do registro    |
| `updatedAt` | Date     | Data da última atualização     |

---

### Hábito (`Habit`)
Representa uma atividade que o usuário quer acompanhar — como “fazer exercícios”, “ler 20 páginas”, etc.

| Campo        | Tipo                     | Descrição                        |
|-------------|-------------------------|----------------------------------|
| `id`        | string (UUID)           | Identificador único do hábito    |
| `userId`    | string                  | ID do usuário dono do hábito     |
| `title`     | string                  | Nome do hábito                   |
| `description` | string?               | Detalhes opcionais sobre o hábito |
| `frequency` | enum (`DAILY`, `WEEKLY`, `MONTHLY`) | Frequência de execução          |
| `createdAt` | Date                    | Data de criação                  |
| `updatedAt` | Date                    | Última atualização               |

---

### Registro de Hábito (`HabitLog`)
Guarda o histórico de quando o usuário completou um hábito.

| Campo       | Tipo                  | Descrição                        |
|------------|----------------------|----------------------------------|
| `id`       | string (UUID)        | Identificador único do registro  |
| `habitId`  | string               | ID do hábito completado           |
| `date`     | Date                 | Dia em que o hábito foi concluído|
| `status`   | enum (`DONE`, `MISSED`) | Indica se o hábito foi cumprido |
| `createdAt`| Date                 | Data do registro                  |

---

## ⚙️ Funcionalidades Principais da API

### 👤 Usuários
- **POST /users/register** → Cria um novo usuário.  
- **POST /users/login** → Faz login e retorna token JWT.  
- **GET /users/profile** → Retorna dados do usuário autenticado.  

### 🪶 Hábitos
- **POST /habits** → Cria um novo hábito vinculado ao usuário logado.  
- **GET /habits** → Retorna todos os hábitos do usuário (limitado pelo rate limiter configurável).  
- **PUT /habits/:id** → Atualiza informações do hábito.  
- **DELETE /habits/:id** → Remove um hábito.  

### 📅 Registros de Hábito
- **POST /habits/:id/logs** → Marca um hábito como “feito” ou “perdido” em determinada data.  
- **GET /habits/:id/logs** → Lista o histórico de registros de um hábito específico.  

### ⏳ Rate Limiter (Carta Desafio)
A API aplica um **limite configurável de requisições por usuário**:

- Pode ser configurado **por minuto** e **por hora**.  
- O valor padrão é, por exemplo, **60 requisições por minuto** e **500 por hora**.  
- Se o limite for ultrapassado, o servidor retorna:

```json
{
  "error": "Too many requests. Try again later."
}
