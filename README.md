# BD_PIZZARIA

-- Cria a tabela de Pizzaiolos --
CREATE TABLE Pizzaiolos (
    ID_Pizzaiolo INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL
);

-- Insere alguns dados de pizzaiolos --
INSERT INTO Pizzaiolos (Nome) VALUES
    ('Lucas'),
    ('Irineu'),
    ('Paulo');

-- Cria a tabela de Ingredientes --
CREATE TABLE Ingredientes (
    ID_Ingrediente INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(150) NOT NULL
);

-- Insere alguns ingredientes --
INSERT INTO Ingredientes (Nome) VALUES
('Queijo'),
('Molho de Tomate'),
('Pepperoni'),
('Mussarela'),
('Cebola'),
('Pimentão');

-- Cria a tabela de Pizzas --
CREATE TABLE Pizzas (
    ID_Pizza INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(50) NOT NULL,
    ID_Pizzaiolo INT,
    InstrucoesPreparo TEXT,
    FOREIGN KEY (ID_Pizzaiolo) REFERENCES Pizzaiolos(ID_Pizzaiolo)
);

-- Insere algumas pizzas --
INSERT INTO Pizzas (Nome, ID_Pizzaiolo, InstrucoesPreparo) VALUES
    ('Pizza de Queijo e Pepperoni', 1, 'Passo 1: Coloque o molho de tomate, Passo 2: Coloque o queijo e pepperoni, Passo 3: Coloque no forno a 200°C por 20 minutos.'),
    ('Pizza de 2 Queijos', 2, 'Passo 1: Coloque o molho de tomate, Passo 2: Coloque o queijo e mussarela, Passo 3: Coloque no forno a 200°C por 20 minutos.'),
    ('Pizza SuperVeg', 3, 'Passo 1: Coloque o molho de tomate, Passo 2: Coloque o queijo, cebola e pimentão, Passo 3: Coloque no forno a 200°C por 20 minutos.');

-- Cria a tabela de relação entre Pizzas e Ingredientes --
CREATE TABLE PizzaIngredientes (
	ID_Pizza INT,
    ID_Ingrediente INT,
    PRIMARY KEY (ID_Pizza,  ID_Ingrediente),
    FOREIGN KEY (ID_Pizza) REFERENCES Pizzas(ID_Pizza),
    FOREIGN KEY ( ID_Ingrediente) REFERENCES Ingredientes( ID_Ingrediente)
);

-- Insere os ingredientes de cada pizza --
INSERT INTO PizzaIngredientes (ID_Pizza, ID_Ingrediente) VALUES
    (1, 1),  -- Queijo na Pizza de Pepperoni
    (1, 2),  -- Molho de Tomate na Pizza de Pepperoni
    (1, 3),  -- Pepperoni na Pizza de Pepperoni
    (2, 1),  -- Queijo na Pizza de Mussarela
    (2, 2),  -- Molho de Tomate na Pizza de Mussarela
    (4, 1),  -- Queijo na Pizza Vegetariana
    (4, 2),  -- Molho de Tomate na Pizza Vegetariana
    (4, 4),  -- Mussarela na Pizza Vegetariana
    (4, 5);  -- Cebola na Pizza Vegetariana

-- Relatório: Todas as pizzas e os pizzaiolos aptos a produzi-las --
SELECT p.Nome AS Pizza, pz.Nome AS Pizzaiolo
FROM Pizzas p
JOIN Pizzaiolos pz ON p.ID_Pizzaiolo = pz.ID_Pizzaiolo;

-- Relatório: Todas as pizzas e seus ingredientes --
SELECT p.Nome AS Pizza, GROUP_CONCAT(i.Nome) AS Ingredientes
FROM Pizzas p
JOIN PizzaIngredientes pi ON p.ID_Pizza = pi.ID_Pizza
JOIN Ingredientes i ON pi.ID_Ingrediente = i.ID_Ingrediente
GROUP BY p.ID_Pizza;

-- Relatório: Todos os ingredientes e as pizzas onde são utilizados --
SELECT i.Nome AS Ingrediente, GROUP_CONCAT(p.Nome) AS Pizzas
FROM Ingredientes i
JOIN PizzaIngredientes pi ON i.ID_Ingrediente = pi.ID_Ingrediente
JOIN Pizzas p ON pi.ID_Pizza = p.ID_Pizza
GROUP BY i.ID_Ingrediente;

-- Relatório: Sabores de todas as pizzas, pizzaiolos e instruções --
SELECT p.Nome AS Pizza, pz.Nome AS Pizzaiolo, p.InstrucoesPreparo
FROM Pizzas p
JOIN Pizzaiolos pz ON p.ID_Pizzaiolo = pz.ID_Pizzaiolo;
