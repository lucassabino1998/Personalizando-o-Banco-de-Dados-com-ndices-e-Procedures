# Personalizando-o-Banco-de-Dados-com-ndices-e-Procedures
Este projeto foi desenvolvido para o desafio de Banco de Dados da formação SQL Specialist (DIO), utilizando o banco company_constraints
Este projeto é a criação de índices e otimização de consultas no cenário de um banco de dados empresarial (company_constraints). Foram criados índices baseados nas colunas mais utilizadas nas consultas: departamento dos empregados (Dno), cidade dos departamentos (Dlocation) e gerente dos departamentos (Mgr_ssn).
O objetivo é garantir performance nas buscas de informações críticas como funcionários por departamento e localização dos departamentos.
As consultas foram otimizadas e as justificativas de cada índice estão documentadas no próprio script SQL.
-- Seleciona o banco
USE company_constraints;

-- Criação de índices baseados nas perguntas:

-- 1. Índice para Dno (departamento) na tabela employee
-- (Justificativa: Vamos consultar número de funcionários por departamento, então Dno precisa ser rápido de buscar)
CREATE INDEX idx_employee_dno ON employee(Dno);

-- 2. Índice para Dnumber (chave primária já tem índice) e Dlocation na tabela dept_locations
-- (Justificativa: Vamos consultar departamentos por cidade, cidade é Dlocation)
CREATE INDEX idx_dept_locations_dlocation ON dept_locations(Dlocation);

-- 3. Índice para Mgr_ssn no departament
-- (Justificativa: consultas que podem pedir detalhes do gerente por departamento)
CREATE INDEX idx_dept_mgr_ssn ON departament(Mgr_ssn);

-- ----------------------
-- Agora, as queries pedidas:
-- ----------------------

-- 1. Qual o departamento com maior número de pessoas?
SELECT 
    d.Dname,
    COUNT(e.Ssn) AS qtd_funcionarios
FROM 
    employee e
JOIN 
    departament d ON e.Dno = d.Dnumber
GROUP BY 
    d.Dname
ORDER BY 
    qtd_funcionarios DESC
LIMIT 1;

-- 2. Quais são os departamentos por cidade?
SELECT 
    d.Dname,
    l.Dlocation
FROM 
    departament d
JOIN 
    dept_locations l ON d.Dnumber = l.Dnumber
ORDER BY 
    l.Dlocation;

-- 3. Relação de empregados por departamento
SELECT 
    d.Dname,
    e.Fname,
    e.Lname
FROM 
    employee e
JOIN 
    departament d ON e.Dno = d.Dnumber
ORDER BY 
    d.Dname, e.Lname;

