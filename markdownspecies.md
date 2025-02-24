##2 1 Instalação da base de dados online

[XAMPP]: (https://www.apachefriends.org/pt_br/index.html) - Software que permite desenvolver aplicações mySQL, Apache e php.
[mySQL Workbench]: (https://www.mysql.com/products/workbench/) - Software ferramenta gráfica de administração e desenvolvimento para o banco de dados mySQL.

##2 2 Backup da estrutura da base de dados
*Guarda uma cópia da estrutura das tabelas, usando o seguinte comando:

C:\XAMPP\mysql\         # mysqldump --no-data -u root species > species.SQL

##2 3 Backup dos dados da base de dados
*Guarda uma cópia dos dados das tabelas, usando o seguinte comando:

C:\XAMPP\mysql\         # mysqldump --no-create-info -u root species > species.SQL

##2 4 Restauro da base de dados
 * Importar o ficheiro de dump no formato.sql com o seguinte comando:
 
 C:\XAMPP\mysql\ 		mysql -u root < species.SQL


###3 Foi criada uma base de dados de espécies presentes no Parque da Paz em MySQL com 6 tabelas relacionadas ao tema espécies, mais concretamente para gerenciar informações sobre especies, habitats, pesquisas, observações, fotografos e coleções de amostras.


###2 Estrutura das Tabelas:

__Especies:__ Informações sobre as espécies biológicas.
__Habitats:__ Informações sobre os habitats onde as espécies podem ser encontradas.
__Pesquisas:__ Informações sobre as pesquisas realizadas sobre as espécies.
__Observações:__ Observações feitas sobre as espécies em diferentes momentos.
__Fotografos:__ Informações sobre os fotografos que estão realizando pesquisas ou observações.
__Colecoes:__ Amostras biológicas colhidas quando foi visitado o parque.

###2 Passo 1: Criação da Base de Dados

/*apagar a database*/
## DROP DATABASE IF EXISTS species;
/* Create the database */
## CREATE DATABASE IF NOT EXISTS species;
/* Switch to the species database */
## USE species;

/* Drop existing tables  */
##DROP TABLE IF EXISTS especies;
##DROP TABLE IF EXISTS habitats;
##DROP TABLE IF EXISTS pesquisas;
##DROP TABLE IF EXISTS observacoes;
##DROP TABLE IF EXISTS fotografos;
##DROP TABLE IF EXISTS colecoes;

###2 Passo 2: Definição das Tabelas

###3 1. Tabela especies
Esta tabela contém informações sobre que tipos de espécies se podem encontrar no parque.

CREATE TABLE especies (
especie_id int auto_increment primary key,
nome_comum varchar(100),
nome_cientifico varchar(100),
classe varchar(50), 
descricao Text
);


###3 2. Tabela habitats
Esta tabela contém informações sobre os habitats das espécies presentes no parque.


create table habitats (
habitat_id int auto_increment primary key, 
nome_habitat varchar(50),
exposicao text
);

###3 3. Tabela fotografos
Esta tabela contém informações sobre os fotografos que pesquisaram ou observaram as especies.

create table fotografos (
fotografo_id int auto_increment primary key,
nome varchar(50),
especialidade varchar(100),
telefone varchar(50)
);

###3 4. Tabela pesquisas
Esta tabela armazena as informações sobre pesquisas feitas sobre as espécies (nem todas as espécies foram pesquisadas).

create table pesquisas (
pesquisa_id int auto_increment primary key,
data_de_pesquisa date,
especie_id int,
fotografo_id int,
	FOREIGN KEY (especie_id) REFERENCES especies(especie_id),
	FOREIGN KEY (fotografo_id) REFERENCES fotografos(fotografo_id)
);

###3 5. Tabela observacoes
Esta tabela armazena as observações feitas sobre as espécies em diferentes momentos no parque.

create table observacoes (
observacao_id int auto_increment primary key,
especie_id int,
fotografo_id int,
data_de_observacao date,
habitat_id int,
    FOREIGN KEY (especie_id) REFERENCES especies(especie_id),
    FOREIGN KEY (fotografo_id) REFERENCES fotografos(fotografo_id),
    FOREIGN KEY (habitat_id) REFERENCES habitats(habitat_id)
);

###3 6. Tabela Colecoes
Esta tabela contém informações sobre as amostras biológicas colhidas para estudo.

create table colecoes (
amostra_id int auto_increment primary key,
especie_id int,
data_de_colheita date,
fotografo_id int,
relatorio text,
	FOREIGN KEY (especie_id) REFERENCES especies(especie_id),
    FOREIGN KEY (fotografo_id) REFERENCES fotografos(fotografo_id)
);


###1 Relacionamentos Entre as Tabelas
**especies → pesquisas:**  Uma espécie pode ser objeto de várias pesquisas.
**especies → observacoes:** Uma espécie pode ser observada diversas vezes.
**habitats → observacoes:** Cada observação pode ocorrer em um habitat específico.
**fotografos → pesquisas:** Um fotografo pode estar associado a várias pesquisas.
**fotografos → observacoes:** Fotografos fazem observações sobre as espécies.
**especies → colecoes:** Uma espécie pode ter várias amostras coletadas para estudo.


###2 Passo 3: Inserção de Dados nas tabelas

###3 1. Inserir dados na tabela fotografos

**insert into** fotografos(nome,especialidade,telefone) **values**
('Mário Estevens','Paisagens','212580081'),('José Barros','Plantas','212580000'),('Ricardo Gameiro','Animais','215689339'),('Joaquim Simão','Objectos','217774444'),('Luis Quinta','Animais','215959367'),
('Albano Soares','Plantas','212590081'),('Rui Félix','Animais','212321233'),('Evgenity Yakhantov','Plantas','212225559'),('Alexis Lours','Paisagens','219997788'),('Mick Talbot','Animais','214567892'),
('Eva Monteiro','Plantas','213214596'),('Gaspar Alves','Objectos','218596473');

###3 2. Inserir dados na tabela habitats

**insert into** habitats(nome_habitat, exposicao) **values**
('Urbano','Habitats urbanos sao aqueles em que existe presença humana'),('Bosque','Um bosque é um habitat com uma vasta zona florestal'),
('Subsolo','Algumas espécies tem a capacidade para viver debaixo da terra'),('Aquatico','Este é um habitat onde existe uma grande quantidade de agua, como um rio, lago ou oceano'),
('Planicie','Habitat terrestre com ausencia de um grande aglomerado de arvores');


###3 3. Inserir dados na tabela especies

**insert into** especies(nome_comum,nome_cientifico,classe,descricao) **values**
('Pombo das Rochas','Columba liva','Aves','Ave comum nas grandes cidades'),('Rola-turca','Streptopelia decaocto','Aves','Ave comum nas grandes cidades'),('Estorninho-preto','Sturnus unicolor','Aves','Ave comum nos bosques'),
('pardal','Passer domesicus','Aves','Ave comum nas grandes cidades'),('Álveola Cinzenta','Motacila cinerea','Aves','Ave comum nos bosques'),('Gincho','Chroicocephalus ridibundus','Aves','Ave comum nos bosques'),
('Cágado mediterrânico','Mauremys leprosa','Réptil','Reptil invasor de meio aquático'),('Cobra de água','Natrix astreptophora','Réptil','Réptil nativo'),
('Rã verde','Pelophylax perezi','Anfibio','Anfibio comum em meio aquatco de parques'),('morcego-anao','Pipistrellus pipistrellus','Mamifero','Mamifero voador'),('Imperador','Anax Imperator','Insecto','Insecto comum dos bosques'),
('Libelinha Anã','Ischnura pumilio','Insecto','Insecto comum dos bosques'),('Tira olhos menor','Anax parthnope','Insecto','Insecto comum dos bosques'),
('Coelho bravo','Oryctolagus cuniculus','Mamifero','Mamifero comum dos bosques'),('Toupeira','Talpa Occidentalis','Mamifero','Mamifero residente no subsolo'),
('Cobra rateira','Malpolon monspessulanus','Reptil','Reptil comum dos bosques'),('Lagartixa do mato','Psammodromus algirus','Reptil','Reptil comum em urbanizações'),('Sardão','Timon Lepidus','Reptil','Reptil comum dos bosques'),
('Vespa do papel','Polistes gallicus','Insecto','Insecto comum em bosques e urbanizações'),('Estevinha','Cistus salviifolius','Planta','Arbusto com flor'),('Rosmanino','Lavandula stoechas','Planta','Planta de flor'),
('Medronheiro','Arbutus unedo','Planta','Arbusto de fruto'),('Carrasco','Quercus coccifera','Planta','Arvore de grande porte'),('Sobreiro','Quercus suber','Planta','Arvore de grande porte'),
('Pinheiro bravo','Pinus pinaster','Planta','Arvore de grande porte'),('Borboleta carnaval','Zerynthia rumina','Insecto','Insecto comum dos bosques'),
('Borboleta zebra','Iphiclides feisthamelii','Insecto','Insecto comum dos bosques'),('Águia de asa redonda','Buteo buteo','Aves','Ave rara dos bosques'),('Poupa','Upupa epops','Aves','Ave raramente avistada de jardim'),
('Abelha do mel','Apis mellifera','Insecto','Insecto muito comum dos bosques'),('Abelhão grande do jardim','Bombus ruderatus','Insecto','Insecto muito comum dos bosques'),
('Abelhão terrestre','Bombus terrestris','Insecto','Insecto muito comum dos bosques'),('Margarida','Bellis perenis','Planta','Planta com flor comum nos bosques');


###3 4. Inserir dados na tabela pesquisas

**insert into** pesquisas(data_de_pesquisa,especie_id,fotografo_id) **values**
('2020/01/01','14','1'),('2020/07/25','28','1'),
('2020/09/25','25','2'),('2020/02/16','30','5'),
('2022/09/26','22','8'),('2021/08/19','10','11'),
('2023/11/26','8','2'),('2024/02/23','6','1'),
('2023/11/10','26','4'),('2024/10/23','33','10');

###3 5. inserir dados na tabela observacoes

**insert into** observacoes(especie_id,fotografo_id,data_de_observacao,habitat_id) **values**
('25','2','2020/12/12','2'),('14','1','2020/05/05','2'),
('1','10','2022/12/23','1'),('25','3','2022/06/22','2'),
('7','3','2022/06/22','4'),('8','3','2022/06/22','4'),
('15','7','2023/08/19','3'),('16','8','2024/08/25','2'),
('22','11','2023/07/19','1'),('31','7','2024/06/30','2');


###3 6. Inserir dados na tabela colecoes_amostras

**insert into** colecoes(especie_id,data_de_colheita,fotografo_id,relatorio) **values**
('2','2024/08/19','1','Apesar de não ter ser avistada a ave em causa, foram colhidas penas'),
('1','2023/10/21','5','Colhidas algumas penas deste exemplar'),
('8','2024/05/25','6','Foi encontrado uma muda de pele deste exemplar'),
('14','2020/05/05','1','Depois de ser avistado, o animal era docil e foi possivel colher um pouco do seu pelo'),
('33','2022/04/04','4','Foram encontradas algumas petalas desta planta'),
('22','2023/11/20','6','Colhidos alguns frutos deste arbusto');
