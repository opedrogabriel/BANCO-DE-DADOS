-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema jogo
-- -----------------------------------------------------
DROP SCHEMA IF EXISTS jogo ;

-- -----------------------------------------------------
-- Schema jogo
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS jogo DEFAULT CHARACTER SET utf8 ;
USE jogo ;

-- -----------------------------------------------------
-- Table jogo.atributos
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.atributos (
  idficha_personagem INT NOT NULL,
  força VARCHAR(45) NULL,
  vitalidade VARCHAR(45) NULL,
  inteligencia VARCHAR(45) NULL,
  agilidade VARCHAR(45) NULL,
  PRIMARY KEY (idficha_personagem))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table jogo.classes
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.classes (
  id_classes INT NOT NULL AUTO_INCREMENT,
  nome VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_classes))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table jogo.usuario
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.usuario (
  id_usuario INT NOT NULL AUTO_INCREMENT,
  nome VARCHAR(45) NOT NULL,
  email VARCHAR(45) NOT NULL,
  senha VARCHAR(45) NOT NULL,
  personagem VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_usuario))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table jogo.personagem
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.personagem (
  id_personagem INT NOT NULL AUTO_INCREMENT,
  nome VARCHAR(45) NOT NULL,
  idade VARCHAR(45) NOT NULL,
  genero VARCHAR(45) NOT NULL,
  atributos_idficha_personagem INT NOT NULL,
  classes_id_classes INT NOT NULL,
  usuario_id_usuario INT NOT NULL,
  PRIMARY KEY (id_personagem),
  INDEX fk_personagem_atributos_idx (atributos_idficha_personagem ASC),
  INDEX fk_personagem_classes1_idx (classes_id_classes ASC),
  INDEX fk_personagem_usuario1_idx (usuario_id_usuario ASC) ,
  CONSTRAINT fk_personagem_atributos
    FOREIGN KEY (atributos_idficha_personagem)
    REFERENCES jogo.atributos (idficha_personagem)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_personagem_classes1
    FOREIGN KEY (classes_id_classes)
    REFERENCES jogo.classes (id_classes)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_personagem_usuario1
    FOREIGN KEY (usuario_id_usuario)
    REFERENCES jogo.usuario (id_usuario)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table jogo.inimigos
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.inimigos (
  id_inimigos INT NOT NULL,
  nome VARCHAR(45) NOT NULL,
  vida VARCHAR(45) NOT NULL,
  poder VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_inimigos))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table jogo.itens
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.itens (
  id_itens INT NOT NULL AUTO_INCREMENT,
  armadura VARCHAR(45) NOT NULL,
  armas VARCHAR(45) NOT NULL,
  vestimentas VARCHAR(45) NOT NULL,
  pocoes VARCHAR(45) NOT NULL,
  personagem_id_personagem INT NOT NULL,
  PRIMARY KEY (id_itens),
  INDEX fk_itens_personagem1_idx (personagem_id_personagem ASC) ,
  CONSTRAINT fk_itens_personagem1
    FOREIGN KEY (personagem_id_personagem)
    REFERENCES jogo.personagem (id_personagem)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table jogo.regioes
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.regioes (
  id_regioes INT NOT NULL AUTO_INCREMENT,
  nome VARCHAR(45) NOT NULL,
  bioma VARCHAR(45) NOT NULL,
  PRIMARY KEY (id_regioes))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table jogo.regioes_inimigos
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogo.regioes_inimigos (
  id_regioes INT NOT NULL,
  id_inimigos INT NOT NULL,
  PRIMARY KEY (id_regioes, id_inimigos),
  INDEX fk_regioes_has_inimigos_inimigos1_idx (id_inimigos ASC) ,
  INDEX fk_regioes_has_inimigos_regioes1_idx (id_regioes ASC) ,
  CONSTRAINT fk_regioes_has_inimigos_regioes1
    FOREIGN KEY (id_regioes)
    REFERENCES jogo.regioes (id_regioes)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_regioes_has_inimigos_inimigos1
    FOREIGN KEY (id_inimigos)
    REFERENCES jogo.inimigos (id_inimigos)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

INSERT INTO `jogo`.`usuario` (`nome`, `email`, `senha`, `personagem`) VALUES
('Alice', 'alice@example.com', 'senha123', 'AliceCharacter'),
('Bob', 'bob@example.com', 'senha456', 'BobCharacter'),
('Charlie', 'charlie@example.com', 'senha789', 'CharlieCharacter'),
('Diana', 'diana@example.com', 'senha321', 'DianaCharacter');

INSERT INTO `jogo`.`atributos` (`idficha_personagem`, `força`, `vitalidade`, `inteligencia`, `agilidade`) VALUES
(1, '20', '30', '25', '15'),
(2, '15', '20', '30', '25'),
(3, '25', '30', '20', '10'),
(4, '18', '22', '28', '20');

INSERT INTO `jogo`.`classes` (`nome`) VALUES
('Guerreiro'),
('Mago'),
('Arqueiro'),
('Ladino');

INSERT INTO `jogo`.`personagem` (`nome`, `idade`, `genero`, `atributos_idficha_personagem`, `classes_id_classes`, `usuario_id_usuario`) VALUES
('Artemis', '25', 'Feminino', 1, 1, 1),
('Merlin', '40', 'Masculino', 2, 2, 2),
('Legolas', '30', 'Masculino', 3, 3, 3),
('Zelda', '20', 'Feminino', 4, 4, 4);

INSERT INTO `jogo`.`inimigos` (`id_inimigos`, `nome`, `vida`, `poder`) VALUES
(1, 'Goblin', '50', '10'),
(2, 'Orc', '100', '20'),
(3, 'Dragão', '500', '50'),
(4, 'Esqueleto', '30', '5');

INSERT INTO `jogo`.`itens` (`id_itens`, `armadura`, `armas`, `vestimentas`, `pocoes`, `personagem_id_personagem`) VALUES
(1, 'Armadura de Couro', 'Espada Curta', 'Capacete', 'Poção de Cura', 1),
(2, 'Manto Mágico', 'Varinha Mágica', 'Túnica', 'Poção de Mana', 2),
(3, 'Armadura Leve', 'Arco e Flecha', 'Botas de Couro', 'Poção de Força', 3);

INSERT INTO `jogo`.`regioes` (`id_regioes`, `nome`, `bioma`) VALUES
(1, 'Floresta Encantada', 'Floresta'),
(2, 'Deserto Ardente', 'Deserto'),
(3, 'Montanhas Geladas', 'Montanha');
	
    INSERT INTO `jogo`.`regioes_inimigos` (`id_regioes`, `id_inimigos`) VALUES
(1, 1),
(1, 2),
(2, 2),
(2, 3),
(3, 3);

select * from classes; 

select i.nome as nome_inimigo from inimigos i join regioes_inimigos ri 
on i.id_inimigos = ri.id_inimigos where ri.id_regioes = 1;

