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
