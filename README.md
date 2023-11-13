# UniversidadeBancoDeDados
banco de dados FACENS semestre 2
1- Crie um banco de dados para armazenar alunos, cursos e professores de uma
universidade;
```SQL
-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema Aula10
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema Aula10
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `Aula10` DEFAULT CHARACTER SET utf8 ;
USE `Aula10` ;

-- -----------------------------------------------------
-- Table `Aula10`.`aluno`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Aula10`.`aluno` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `ra` VARCHAR(45) NULL,
  `nome` VARCHAR(100) NULL,
  `sobrenome` VARCHAR(100) NULL,
  `email` VARCHAR(100) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Aula10`.`professores`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Aula10`.`professores` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `nome` VARCHAR(45) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Aula10`.`curso`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Aula10`.`curso` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `disciplina` VARCHAR(100) NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Aula10`.`professores_has_curso`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Aula10`.`professores_has_curso` (
  `professores_id` INT NOT NULL,
  `curso_id` INT NOT NULL,
  PRIMARY KEY (`professores_id`, `curso_id`),
  INDEX `fk_professores_has_curso_curso1_idx` (`curso_id` ASC) VISIBLE,
  INDEX `fk_professores_has_curso_professores_idx` (`professores_id` ASC) VISIBLE,
  CONSTRAINT `fk_professores_has_curso_professores`
    FOREIGN KEY (`professores_id`)
    REFERENCES `Aula10`.`professores` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_professores_has_curso_curso1`
    FOREIGN KEY (`curso_id`)
    REFERENCES `Aula10`.`curso` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `Aula10`.`aluno_has_curso`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `Aula10`.`aluno_has_curso` (
  `aluno_id` INT NOT NULL,
  `curso_id` INT NOT NULL,
  PRIMARY KEY (`aluno_id`, `curso_id`),
  INDEX `fk_aluno_has_curso_curso1_idx` (`curso_id` ASC) VISIBLE,
  INDEX `fk_aluno_has_curso_aluno1_idx` (`aluno_id` ASC) VISIBLE,
  CONSTRAINT `fk_aluno_has_curso_aluno1`
    FOREIGN KEY (`aluno_id`)
    REFERENCES `Aula10`.`aluno` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_aluno_has_curso_curso1`
    FOREIGN KEY (`curso_id`)
    REFERENCES `Aula10`.`curso` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;


```


2- Faça a modelagem do banco e identifique as entidades, seus atributos e relacionamentos;
![image](https://github.com/huankzera/UniversidadeBancoDeDados/assets/126423433/e6078f89-0cd0-4052-8b59-3ca3a7f7a3d6)

3- Crie o modelo físico do banco de dados (script SQL);
![image](https://github.com/huankzera/UniversidadeBancoDeDados/assets/126423433/8a988440-2d65-4b8b-8576-686228751ee9)

4- Utilize Stored Procedures para automatizar a inserção e seleção dos cursos;
### Criando Procedures
```SQL
DELIMITER $
create procedure Insert_Curso(
disciplina varchar(100)
)
begin 
	insert into curso (disciplina) values (disciplina);
end $
DELIMITER;
```
```SQL
DELIMITER $
create procedure Select_Cursos()
begin
	select * from curso;
end $
DELIMITER ;
```
### Utilizando função Call para inserção de valores e chamada de função
```SQL
call Insert_Curso ('Analise e desenvolvimento de Sistemas');
call Insert_Curso ('Engenharia');
call Insert_Curso ('Enfermagem');
call Insert_Curso  ('Banco de Dados');
```
```SQL
call Select_Cursos();call Select_Cursos();
```
![image](https://github.com/huankzera/UniversidadeBancoDeDados/assets/126423433/6fbd84da-13d4-400f-98d2-d714b27c67db)

###  Comando criado para auto preenchimento de e-mail
```SQL
DELIMITER $$
CREATE TRIGGER GenerateEmail
BEFORE INSERT ON Aula10.aluno
FOR EACH ROW
BEGIN
  DECLARE email_count INT;
  SET email_count = 0;
  
  -- Verifique se já existe um e-mail com o mesmo nome e sobrenome
  SELECT COUNT(*) INTO email_count FROM Aula10.aluno WHERE email = CONCAT(NEW.nome, '.', NEW.sobrenome, '@facens.com');
  
  -- Se houver um conflito, adicione um número incremental ao e-mail
  IF email_count > 0 THEN
    SET NEW.email = CONCAT(NEW.nome, '.', NEW.sobrenome, email_count, '@facens.com');
  ELSE
    SET NEW.email = CONCAT(NEW.nome, '.', NEW.sobrenome, '@facens.com');
  END IF;
END;
$$
DELIMITER ;

```
### Insert dos valores na tabela Aluno
```SQL
INSERT INTO Aula10.aluno (ra, nome, sobrenome) VALUES ('236058','Teruo', 'Yamassaka');
INSERT INTO Aula10.aluno (ra, nome, sobrenome) VALUES ('236057','Yasmin', 'Braz');
INSERT INTO Aula10.aluno (ra, nome, sobrenome) VALUES ('236056','William', 'Santos');
INSERT INTO Aula10.aluno (ra, nome, sobrenome) VALUES ('236055','Juliana', 'Ferreira');
```


