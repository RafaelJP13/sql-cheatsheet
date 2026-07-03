# SQL Utility List

Olá! Este é um material de estudo sobre **SQL e MySQL**, utilizando o
MySQL Query Browser e o XAMPP como ferramentas de apoio.

## Sumário

- [Dicionário de comandos](#dicionário-de-comandos)
- [Modelo Entidade Relacionamento (MER)](#modelo-entidade-relacionamento-mer)
- [Inserção de dados](#inserção-de-dados)
- [Comando de alteração](#comando-de-alteração)
- [Comando de seleção](#comando-de-seleção)
- [Stored Procedures](#stored-procedures-procedimentos-armazenados)

---

## Dicionário de comandos

```sql
-- EXCLUIR COLUNA
ALTER TABLE nome_tabela DROP COLUMN nome_coluna

-- ADICIONAR COLUNA
ALTER TABLE nome_tabela ADD COLUMN nome_coluna TIPO

-- ALTERAR COLUNA
ALTER TABLE nome_tabela MODIFY COLUMN nome_coluna TIPO

-- EXCLUIR TABELA
DROP TABLE nome_tabela

-- LIMITAR SELECT
SELECT nome_campos FROM nome_tabela LIMIT numero_limite

-- INSERIR DADOS
INSERT INTO nome_tabela VALUES (valores)

-- EXCLUINDO DADOS
TRUNCATE TABLE nome_tabela

-- ATUALIZANDO DADOS
UPDATE nome_tabela SET nome_coluna = novo_valor

-- SELECIONANDO DADOS
SELECT nome_coluna FROM nome_tabela

-- SELECIONANDO DADOS SEM REPETIR
SELECT DISTINCT nome_coluna_distinguida FROM nome_tabela

-- SELECIONANDO DADOS E ORGANIZANDO
SELECT nome_coluna FROM nome_tabela ORDER BY nome_coluna

-- SELECIONANDO COM WHERE
SELECT nome_coluna FROM nome_tabela WHERE condição

-- SELECIONANDO E COM CLÁUSULA AND E OR
SELECT nome_coluna FROM nome_tabela WHERE condição AND/OR condição

-- SELECIONANDO CAMPOS DE TABELAS DIFERENTES (tipos de dados tem que ser iguais)
SELECT nome_coluna FROM nome_tabela UNION / UNION ALL SELECT nome_coluna2 FROM tb_tabela2

-- CRIANDO TABELA E INSERINDO DADOS DE UMA SELEÇÃO
CREATE TABLE nome_nova_tabela SELECT nome_campo FROM nome_tabela

-- SELECIONAR O MENOR NUMERO DE UMA LISTA
SELECT MIN(coluna_numeros) FROM nome_tabela

-- SELECIONAR O MAIOR NUMERO DE UMA LISTA
SELECT MAX(coluna_numeros) FROM nome_tabela

-- SELECIONAR QUANTOS CAMPOS TEM REGISTRADO
SELECT COUNT(nome_coluna) FROM nome_tabela

-- SELECIONAR MEDIA
SELECT AVG(coluna_numeros) FROM nome_tabela

-- MOSTRAR SOMA
SELECT SUM(coluna_numeros) FROM nome_tabela

-- SELECIONANDO DADOS COMUNS EM 2 TABELAS DIFERENTES
SELECT nome_campo FROM primeira_tabela INNER JOIN segunda_tabela ON
tabela1.campoComum = tabela2.campoComum

-- SELECIONANDO DADOS DA TABELA ESQUERDA MESMO QUE NÃO TENHA EM COMUM COM A TABELA DA DIREITA
SELECT nome_campo FROM primeira_tabela LEFT JOIN segunda_tabela ON
tabela1.campoComum = tabela2.campoComum

-- SELECIONANDO DADOS DA TABELA DIREITA MESMO QUE NÃO TENHA EM COMUM COM A TABELA DA ESQUERDA
SELECT nome_campo FROM primeira_tabela RIGHT JOIN segunda_tabela ON
tabela1.campoComum = tabela2.campoComum

-- SELECIONANDO DADOS DA TABELA DIREITA E DA ESQUERDA MESMO QUE NAO SEJAM COMUNS
SELECT nome_campo FROM primeira_tabela FULL JOIN segunda_tabela ON
tabela1.campoComum = tabela2.campoComum

-- SELEÇÃO DE DADOS QUE ESTÃO JA DETERMINADOS NA PESQUISA
SELECT nome_campos FROM nome_tabela WHERE nome_campo IN ou NOT IN (Valores)

-- CRIANDO PROCEDURE
DELIMITER $$

    CREATE PROCEDURE nomeProcedure(parametros)
    BEGIN
        CODIGO
    END$$

DELIMITER;

-- RENOMEANDO TABELA
RENAME TABLE nome_tabela TO novo_nome_tabela

-- SELECIONANDO REGISTROS POR NOME
SELECT nome_coluna FROM nome_tabela WHERE LIKE/NOT LIKE 'String comparativa'/'_string comparativa'/'% string comparativa'

-- SELECIONANDO REGISTROS POR PADRÕES DE LETRAS
SELECT nome_coluna FROM nome_tabela REGEXP '^[letras]'/'[letras]'/'[letras]$'
```

---

## Modelo Entidade Relacionamento (MER)

> Script de criação do banco de dados `db_MommyHealth`.

```sql
CREATE DATABASE db_MommyHealth;

USE db_MommyHealth;

CREATE TABLE tb_tipoMedico(

  IDtipoMedico INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  nomeTipoMedico VARCHAR(20)
);

CREATE TABLE tb_tipoAcompanhante(

  IDtipoAcompanhante INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  nomeTipoAcompanhante VARCHAR(20)
);

CREATE TABLE tb_medico(

  IDmedico INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  IDtipoMedico INT NOT NULL,
  nomeMedico VARCHAR(30),
  sobrenomeMedico VARCHAR(60),
  emailMedico VARCHAR(50),
  FOREIGN KEY (IDtipoMedico) REFERENCES tb_tipoMedico(IDtipoMedico)
);

CREATE TABLE tb_usuario(

  IDusuario INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  IDmedico INT NOT NULL,
  nomeUsuario VARCHAR(30),
  sobrenomeUsuario VARCHAR(60),
  dataNascimento DATE,
  senha VARCHAR(12),
  meses CHAR(1),
  imagem VARCHAR(255),
  email VARCHAR(50),
  rua VARCHAR(45),
  cep CHAR(9),
  cidade VARCHAR(45),
  uf CHAR(2),
  FOREIGN KEY (IDmedico) REFERENCES tb_medico(IDmedico)
);

CREATE TABLE tb_contatoMovel(

  IDcontatoMovel INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  IDusuario INT NOT NULL,
  telefone VARCHAR(12),
  celular VARCHAR(12),
  FOREIGN KEY (IDusuario) REFERENCES tb_usuario(IDusuario)
);

CREATE TABLE tb_exame(

  IDexame INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  IDusuario INT NOT NULL,
  nome VARCHAR(20),
  descricao VARCHAR(200),
  datae DATE,
  FOREIGN KEY IDusuario REFERENCES tb_usuario(IDusuario)
);

CREATE TABLE tb_diario(

  IDdiario INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  IDusuario INT NOT NULL,
  meses CHAR(2),
  temperatura CHAR(2),
  pressao VARCHAR(3),
  medicamentos VARCHAR(255),
  examesFeitos VARCHAR(255),
  sensacao VARCHAR(255),
  FOREIGN KEY IDusuario REFERENCES tb_usuario(IDusuario)
);

CREATE TABLE tb_acompanhante(

  IDacompanhante INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  IDusuario INT NOT NULL,
  IDtipoAcompanhante INT NOT NULL,
  nome VARCHAR(50),
  email VARCHAR(50),
  FOREIGN KEY IDusuario REFERENCES tb_usuario(IDusuario),
  FOREIGN KEY IDtipoAcompanhante REFERENCES tb_tipoAcompanhante(IDtipoAcompanhante)
);
```

---

## Inserção de dados

```sql
INSERT INTO nome_tabela VALUES (valores)
```

```sql
INSERT INTO tb_tipoAcompanhante VALUES
(DEFAULT,'Avó'),
(DEFAULT,'Avô'),
(DEFAULT,'Cunhada'),
(DEFAULT,'Cunhado'),
(DEFAULT,'Esposa'),
(DEFAULT,'Esposo'),
(DEFAULT,'Filha'),
(DEFAULT,'Filho'),
(DEFAULT,'Irmã'),
(DEFAULT,'Irmão'),
(DEFAULT,'Madrasta'),
(DEFAULT,'Madrinha'),
(DEFAULT,'Mãe'),
(DEFAULT,'Neta'),
(DEFAULT,'Neto'),
(DEFAULT,'Padrasto'),
(DEFAULT,'Pai'),
(DEFAULT,'Prima'),
(DEFAULT,'Primo'),
(DEFAULT,'Sobrinha'),
(DEFAULT,'Sobrinho'),
(DEFAULT,'Sogra'),
(DEFAULT,'Sogro'),
(DEFAULT,'Tia'),
(DEFAULT,'Tio');

INSERT INTO tb_medico VALUES
(DEFAULT, 31, 'Fernando', 'Henrique', 'FeHenrique@hotmail.com'),
(DEFAULT, 32, 'Fernanda', 'Miriam', 'FernandaMiriam@gmail.com'),
(DEFAULT, 32, 'Pedro', 'Moreira de Souza', 'MoreiraSouza@gmail.com'),
(DEFAULT, 33, 'Matheus', 'Oliveira da Silva', 'MateusOli@hotmail.com'),
(DEFAULT, 34, 'Vinicius', 'Fernandes da Silva', 'ViniFS@bol.com'),
(DEFAULT, 35, 'Flavio', 'Martins', 'FlavinhoMartins@hotmail.com'),
(DEFAULT, 36, 'Donavan', 'Pereira', 'DodoPereira@hotmail.com');

INSERT INTO tb_usuario VALUES
(DEFAULT,5,'Rochele','Moreira Garcia','1999-01-02','123456','0','img','RocheleGarcia@hotmail.com','Rua aviador edu Pereira','11326080','São Paulo','SP'),
(DEFAULT,6,'Miriam','Ferreira de Lima','1998-02-03','645231','1','img2','MiriamLima@hotmail.com','Avenida Monsales Garcia','12835458','Minas Gerais','MG'),
(DEFAULT,4,'Roberta','D'' Assis Lima','1997-04-05','1252455','0','img3','Robertinha@hotmail.com','Avenida Rio Branco','83545255','São Paulo','SP'),
(DEFAULT,5,'Fabiana','Silva Oliveira','1995-05-05','835211','3','img4','FabiSilva@hotmail.com','Rua Professor Aprígio Gonzaga','352353','São Paulo','SP');

INSERT INTO tb_exame VALUES
(DEFAULT,13,'Exame de Sangue','Coletar o sangue para conseguir informações relevantes','2018-02-02'),
(DEFAULT,20,'Exame de Fezes','Coletar as fazes para conseguir informações rleevantes','2018-03-05'),
(DEFAULT,23,'Exame do Pezinho','Verificar pé do bebe para saber saude de determinada area','2018-07-08');
```

> ⚠️ **Atenção:** o uso do `AUTO_INCREMENT` na coluna `PRIMARY KEY` faz os
> números variarem de computador para computador. Portanto, verifique quais
> são os códigos da chave primária da tabela `TipoAcompanhante`, pois podem
> ser diferentes dos usados aqui (que começam em 31, 32...).

---

## Comando de alteração

```sql
ALTER TABLE nome_tabela OPÇÃO nome_coluna TIPO
```

```sql
-- Adicionando uma coluna na tabela
ALTER TABLE tb_contatoMovel ADD COLUMN colunaTeste CHAR(8);

-- Removendo uma coluna na tabela
ALTER TABLE tb_contatoMovel DROP COLUMN colunaTeste;

-- Alterando tipo de uma coluna
ALTER TABLE tb_contatoMovel MODIFY celular VARCHAR(20);

-- Excluindo tabela
DROP TABLE tb_contatoMovel;

-- Alterando dados
UPDATE tb_tipoMedico SET nomeTipoMedico = 'Novo dado' WHERE IDtipoMedico = 31;

-- Excluindo todos os dados
TRUNCATE TABLE tb_tipoMedico;
```

---

## Comando de seleção

```sql
SELECT nome_coluna FROM nome_tabela
```

```sql
-- Selecione tudo da tabela tipoacompanhante
SELECT * FROM tb_tipoacompanhante;

-- Selecione apenas o nome dos acompanhantes
SELECT nomeTipoAcompanhante FROM tb_tipoacompanhante;

-- Selecione apenas os primeiros 5 tipos
SELECT nomeTipoAcompanhante FROM tb_tipoacompanhante LIMIT 5;

-- Selecione com uma condição, apenas os acompanhantes que tem numero ID de 4 à 16 com cláusula AND
SELECT IDtipoAcompanhante, nomeTipoAcompanhante FROM tb_tipoacompanhante WHERE IDtipoAcompanhante > 3 AND IDtipoAcompanhante < 17;

-- Selecionando e organizando por prioridade [ASC -> ascendente] [DESC -> descendente]
SELECT * FROM tb_tipomedico ORDER BY nomeTipoMedico ASC;

-- SELECIONAR O MAIOR NUMERO DE UMA LISTA
SELECT MAX(id_autor) FROM tb_autores;

-- SELECIONAR O MENOR NUMERO DE UMA LISTA
SELECT MIN(id_autor) FROM tb_autores;

-- SELECIONAR MEDIA
SELECT AVG(id_autor) FROM tb_autores;

-- MOSTRAR SOMA
SELECT SUM(id_autor) FROM tb_autores;

-- SELECIONAR QUANTOS CAMPOS TEM REGISTRADO
SELECT COUNT(*) FROM tb_autores;

-- Selecionar o nome dos usuarios e seus respectivos exames
SELECT nomeUsuario, nome FROM tb_usuario INNER JOIN tb_exame ON
tb_usuario.IDusuario = tb_exame.IDusuario;
```

---

## Stored Procedures (Procedimentos Armazenados)

Stored Procedures são pedaços de comandos que podem ser chamados e vão
executar uma atividade automática. Por exemplo: temos um site que busca o
nome de todos os médicos cadastrados no banco. Em vez de usar
`SELECT * FROM tb_medico` direto no código PHP, criamos uma stored procedure
dentro do banco que é chamada no PHP através de, por exemplo,
`CALL procedureNomes(parametros)`. Stored procedures deixam o software mais
rápido porque a lógica já está direto no banco.

### Mostrando nome de todos os médicos

```sql
DELIMITER $$

CREATE PROCEDURE mostrarNomes()

  BEGIN
    SELECT nomeMedico FROM tb_medico;
  END$$

DELIMITER;

CALL mostrarNomes();
```

### Mostrar nome de uma quantidade determinada de médicos

```sql
CREATE PROCEDURE mostrarNomes2(IN mostrarQuantidade TINYINT)

  BEGIN
    SELECT nomeMedico FROM tb_medico LIMIT mostrarQuantidade;
  END$$

DELIMITER;

CALL mostrarNomes2(3);
```

### Excluindo Procedures

```sql
DROP PROCEDURE mostrarNomes2;
DROP PROCEDURE mostrarNomes;
```