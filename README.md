 - *Tabelas:*
     - *Usuários (Pacientes):*
       sql
       CREATE TABLE Usuarios (
           Id INT PRIMARY KEY IDENTITY(1,1),
           Nome NVARCHAR(100) NOT NULL,
           Email NVARCHAR(100) UNIQUE NOT NULL,
           Senha NVARCHAR(100) NOT NULL,
           DataNascimento DATE,
           Genero NVARCHAR(20),
           Telefone NVARCHAR(20)
       );
       
     - *Profissionais de Saúde:*
       sql
       CREATE TABLE Profissionais (
           Id INT PRIMARY KEY IDENTITY(1,1),
           Nome NVARCHAR(100) NOT NULL,
           Email NVARCHAR(100) UNIQUE NOT NULL,
           Senha NVARCHAR(100) NOT NULL,
           Especialidade NVARCHAR(100) NOT NULL,
           CRM NVARCHAR(20) UNIQUE,
           Telefone NVARCHAR(20)
       );
       
     - *Consultas:*
       sql
       CREATE TABLE Consultas (
           Id INT PRIMARY KEY IDENTITY(1,1),
           PacienteId INT FOREIGN KEY REFERENCES Usuarios(Id),
           ProfissionalId INT FOREIGN KEY REFERENCES Profissionais(Id),
           DataHora DATETIME NOT NULL,
           TipoConsulta NVARCHAR(50) NOT NULL, -- Presencial ou Virtual
           Status NVARCHAR(20) DEFAULT 'agendada' -- agendada, concluída, cancelada
       );
       
     - *Dados de Saúde (Monitoramento):*
       sql
       CREATE TABLE DadosSaude (
           Id INT PRIMARY KEY IDENTITY(1,1),
           UsuarioId INT FOREIGN KEY REFERENCES Usuarios(Id),
           Peso DECIMAL(5,2),
           PressaoArterial NVARCHAR(20),
           Glicemia DECIMAL(5,2),
           DataRegistro DATETIME DEFAULT GETDATE()
       );
Passo 1: Banco de Dados (SQL Server), você precisa configurar o banco de dados que será usado para armazenar todas as informações do projeto **Saúde Total*. Vou te guiar detalhadamente sobre o que precisa ser feito:

---

### *Passo 1: Banco de Dados (SQL Server)*

#### 1. *Instalar o SQL Server*
   - Se você ainda não tem o SQL Server instalado, siga os passos abaixo:
     1. Baixe o *SQL Server Express* (versão gratuita) no site oficial da Microsoft:  
        [Download SQL Server Express](https://www.microsoft.com/pt-br/sql-server/sql-server-downloads)
     2. Siga o assistente de instalação e escolha as opções padrão.
     3. Durante a instalação, anote o *nome do servidor* (geralmente localhost ou .\SQLEXPRESS) e defina uma *senha* para o usuário sa (administrador do banco de dados).

---

#### 2. *Instalar o SQL Server Management Studio (SSMS)*
   - O SSMS é uma ferramenta gráfica para gerenciar o SQL Server.
   - Baixe e instale o SSMS:  
     [Download SSMS](https://docs.microsoft.com/pt-br/sql/ssms/download-sql-server-management-studio-ssms)
   - Após a instalação, abra o SSMS e conecte-se ao SQL Server usando o nome do servidor e as credenciais que você configurou durante a instalação.

---

#### 3. *Criar o Banco de Dados*
   - No SSMS, siga os passos para criar um novo banco de dados:
     1. Clique com o botão direito em *Bancos de Dados* no painel esquerdo.
     2. Selecione *Novo Banco de Dados*.
     3. Dê o nome de SaudeTotal e clique em *OK*.

---

#### 4. *Criar as Tabelas*
   - Agora, você precisa criar as tabelas que armazenarão os dados do projeto. Abra uma nova janela de consulta no SSMS e execute os seguintes comandos SQL:

   sql
   USE SaudeTotal; -- Seleciona o banco de dados criado

   -- Tabela de Usuários (Pacientes)
   CREATE TABLE Usuarios (
       Id INT PRIMARY KEY IDENTITY(1,1), -- ID autoincrementável
       Nome NVARCHAR(100) NOT NULL,
       Email NVARCHAR(100) UNIQUE NOT NULL,
       Senha NVARCHAR(100) NOT NULL,
       DataNascimento DATE,
       Genero NVARCHAR(20),
       Telefone NVARCHAR(20)
   );

   -- Tabela de Profissionais de Saúde
   CREATE TABLE Profissionais (
       Id INT PRIMARY KEY IDENTITY(1,1),
       Nome NVARCHAR(100) NOT NULL,
       Email NVARCHAR(100) UNIQUE NOT NULL,
       Senha NVARCHAR(100) NOT NULL,
       Especialidade NVARCHAR(100) NOT NULL,
       CRM NVARCHAR(20) UNIQUE,
       Telefone NVARCHAR(20)
   );

   -- Tabela de Consultas
   CREATE TABLE Consultas (
       Id INT PRIMARY KEY IDENTITY(1,1),
       PacienteId INT FOREIGN KEY REFERENCES Usuarios(Id), -- Chave estrangeira para Usuários
       ProfissionalId INT FOREIGN KEY REFERENCES Profissionais(Id), -- Chave estrangeira para Profissionais
       DataHora DATETIME NOT NULL,
       TipoConsulta NVARCHAR(50) NOT NULL, -- Presencial ou Virtual
       Status NVARCHAR(20) DEFAULT 'agendada' -- agendada, concluída, cancelada
   );

   -- Tabela de Dados de Saúde (Monitoramento)
   CREATE TABLE DadosSaude (
       Id INT PRIMARY KEY IDENTITY(1,1),
       UsuarioId INT FOREIGN KEY REFERENCES Usuarios(Id), -- Chave estrangeira para Usuários
       Peso DECIMAL(5,2), -- Peso do usuário
       PressaoArterial NVARCHAR(20), -- Pressão arterial
       Glicemia DECIMAL(5,2), -- Nível de glicemia
       DataRegistro DATETIME DEFAULT GETDATE() -- Data e hora do registro
   );
   

   - Essas tabelas cobrem as principais funcionalidades do projeto:
     - *Usuários:* Armazena os dados dos pacientes.
     - *Profissionais:* Armazena os dados dos profissionais de saúde.
     - *Consultas:* Armazena os agendamentos de consultas.
     - *DadosSaude:* Armazena os dados de saúde monitorados pelos usuários.

---

#### 5. *Inserir Dados de Teste*
   - Para testar o banco de dados, você pode inserir alguns dados de exemplo:
   sql
   -- Inserir um usuário (paciente)
   INSERT INTO Usuarios (Nome, Email, Senha, DataNascimento, Genero, Telefone)
   VALUES ('João Silva', 'joao@example.com', 'senha123', '1990-01-01', 'Masculino', '11999999999');

   -- Inserir um profissional de saúde
   INSERT INTO Profissionais (Nome, Email, Senha, Especialidade, CRM, Telefone)
   VALUES ('Dra. Maria Souza', 'maria@example.com', 'senha123', 'Cardiologia', '123456', '11988888888');

   -- Inserir uma consulta
   INSERT INTO Consultas (PacienteId, ProfissionalId, DataHora, TipoConsulta)
   VALUES (1, 1, '2024-12-01 10:00:00', 'Presencial');

   -- Inserir dados de saúde
   INSERT INTO DadosSaude (UsuarioId, Peso, PressaoArterial, Glicemia)
   VALUES (1, 70.5, '120/80', 95.0);
   

---

#### 6. *Verificar as Tabelas e Dados*
   - Para verificar se as tabelas e os dados foram criados corretamente, execute as seguintes consultas:
   sql
   -- Listar todos os usuários
   SELECT * FROM Usuarios;

   -- Listar todos os profissionais
   SELECT * FROM Profissionais;

   -- Listar todas as consultas
   SELECT * FROM Consultas;

   -- Listar todos os dados de saúde
   SELECT * FROM DadosSaude;
   

---

### *Resumo do Passo 1*
1. *Instalar o SQL Server e o SSMS.*
2. *Criar o banco de dados SaudeTotal.*
3. *Criar as tabelas* (Usuarios, Profissionais, Consultas, DadosSaude).
4. *Inserir dados de teste* para verificar se tudo está funcionando.
5. *Verificar as tabelas e dados* com consultas SQL.

---

### *Próximos Passos*
- Após configurar o banco de dados, você pode prosseguir para o *Passo 2: Backend (ASP.NET Core Web API)*, onde vamos conectar o banco de dados ao backend e criar a API para o projeto.
