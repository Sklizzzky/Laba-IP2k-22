-- DROP SCHEMA laba;

CREATE SCHEMA laba AUTHORIZATION user24;
-- laba.insurance definition

-- Drop table

-- DROP TABLE laba.insurance;

CREATE TABLE laba.insurance (
	id_ins int4 NOT NULL,
	title text NULL,
	inn int4 NULL,
	address text NULL,
	rs int4 NULL,
	bik int4 NULL,
	CONSTRAINT insurance_pk PRIMARY KEY (id_ins)
);


-- laba.policy_type definition

-- Drop table

-- DROP TABLE laba.policy_type;

CREATE TABLE laba.policy_type (
	id_type_pol int4 NOT NULL,
	title text NULL,
	CONSTRAINT policy_type_pk PRIMARY KEY (id_type_pol)
);


-- laba."role" definition

-- Drop table

-- DROP TABLE laba."role";

CREATE TABLE laba."role" (
	id_role int4 NOT NULL,
	title text NULL,
	CONSTRAINT role_pk PRIMARY KEY (id_role)
);


-- laba.service definition

-- Drop table

-- DROP TABLE laba.service;

CREATE TABLE laba.service (
	id_ser int4 NOT NULL,
	title varchar NULL,
	price numeric NULL,
	code int4 NULL,
	term date NULL,
	CONSTRAINT service_pk PRIMARY KEY (id_ser)
);


-- laba.status definition

-- Drop table

-- DROP TABLE laba.status;

CREATE TABLE laba.status (
	id_stat int4 NOT NULL,
	stat_name text NULL,
	CONSTRAINT status_pk PRIMARY KEY (id_stat)
);


-- laba.type_lab definition

-- Drop table

-- DROP TABLE laba.type_lab;

CREATE TABLE laba.type_lab (
	id_type_lab int4 NOT NULL,
	"type" int4 NULL,
	CONSTRAINT type_lab_pk PRIMARY KEY (id_type_lab)
);


-- laba."order" definition

-- Drop table

-- DROP TABLE laba."order";

CREATE TABLE laba."order" (
	id_ord int4 NOT NULL,
	ord_date date NULL,
	id_stat int4 NOT NULL,
	CONSTRAINT order_pk PRIMARY KEY (id_ord),
	CONSTRAINT order_fk FOREIGN KEY (id_stat) REFERENCES laba.status(id_stat)
);


-- laba."user" definition

-- Drop table

-- DROP TABLE laba."user";

CREATE TABLE laba."user" (
	id_user int4 NOT NULL,
	"name" text NULL,
	login varchar NULL,
	"password" varchar NULL,
	ip varchar NULL,
	lastenter date NULL,
	id_role int4 NOT NULL,
	CONSTRAINT user_pk PRIMARY KEY (id_user),
	CONSTRAINT user_fk FOREIGN KEY (id_role) REFERENCES laba."role"(id_role)
);


-- laba.accountant definition

-- Drop table

-- DROP TABLE laba.accountant;

CREATE TABLE laba.accountant (
	id_acc int4 NOT NULL,
	acc_name text NULL,
	id_user int4 NOT NULL,
	CONSTRAINT accountant_pk PRIMARY KEY (id_acc),
	CONSTRAINT accountant_fk FOREIGN KEY (id_user) REFERENCES laba."user"(id_user)
);


-- laba.labarotory_assistant definition

-- Drop table

-- DROP TABLE laba.labarotory_assistant;

CREATE TABLE laba.labarotory_assistant (
	id_lab_ass int4 NOT NULL,
	id_user int4 NOT NULL,
	id_type_lab int4 NOT NULL,
	CONSTRAINT labarotory_assistant_pk PRIMARY KEY (id_lab_ass),
	CONSTRAINT labarotory_assistant_fk FOREIGN KEY (id_user) REFERENCES laba."user"(id_user),
	CONSTRAINT labarotory_assistant_fk1 FOREIGN KEY (id_type_lab) REFERENCES laba.type_lab(id_type_lab)
);


-- laba.ord_ser definition

-- Drop table

-- DROP TABLE laba.ord_ser;

CREATE TABLE laba.ord_ser (
	id_ord_ser int4 NOT NULL,
	id_ser int4 NOT NULL,
	id_ord int4 NOT NULL,
	id_stat int4 NOT NULL,
	CONSTRAINT ord_ser_pk PRIMARY KEY (id_ord_ser),
	CONSTRAINT ord_ser_fk FOREIGN KEY (id_ser) REFERENCES laba.service(id_ser),
	CONSTRAINT ord_ser_fk1 FOREIGN KEY (id_ord) REFERENCES laba."order"(id_ord),
	CONSTRAINT ord_ser_fk2 FOREIGN KEY (id_stat) REFERENCES laba.status(id_stat)
);


-- laba.patient definition

-- Drop table

-- DROP TABLE laba.patient;

CREATE TABLE laba.patient (
	id_pat int4 NOT NULL,
	"name" text NULL,
	birth date NULL,
	pass_s int4 NULL,
	pass_n int4 NULL,
	tel_num int4 NULL,
	email text NULL,
	id_user int4 NOT NULL,
	CONSTRAINT patient_pk PRIMARY KEY (id_pat),
	CONSTRAINT patient_fk FOREIGN KEY (id_user) REFERENCES laba."user"(id_user)
);


-- laba.ser_com definition

-- Drop table

-- DROP TABLE laba.ser_com;

CREATE TABLE laba.ser_com (
	id_com_ser int4 NOT NULL,
	id_ser int4 NOT NULL,
	id_lab_ass int4 NOT NULL,
	id_pat int4 NOT NULL,
	analyzer int4 NOT NULL,
	id_ord_ser int4 NOT NULL,
	CONSTRAINT ser_com_pk PRIMARY KEY (id_com_ser),
	CONSTRAINT ser_com_fk FOREIGN KEY (id_ord_ser) REFERENCES laba.ord_ser(id_ord_ser),
	CONSTRAINT ser_com_fk_1 FOREIGN KEY (id_ser) REFERENCES laba.service(id_ser),
	CONSTRAINT ser_com_fk_2 FOREIGN KEY (id_lab_ass) REFERENCES laba.labarotory_assistant(id_lab_ass),
	CONSTRAINT ser_com_fk_3 FOREIGN KEY (id_pat) REFERENCES laba.patient(id_pat)
);


-- laba."Policy" definition

-- Drop table

-- DROP TABLE laba."Policy";

CREATE TABLE laba."Policy" (
	id_pol int4 NOT NULL,
	id_pat int4 NOT NULL,
	id_ins int4 NULL,
	pol_num int4 NULL,
	id_type_pol int4 NOT NULL,
	CONSTRAINT policy_pk PRIMARY KEY (id_pol),
	CONSTRAINT policy_fk FOREIGN KEY (id_pat) REFERENCES laba.patient(id_pat),
	CONSTRAINT policy_fk_1 FOREIGN KEY (id_ins) REFERENCES laba.insurance(id_ins),
	CONSTRAINT policy_fk_2 FOREIGN KEY (id_type_pol) REFERENCES laba.policy_type(id_type_pol)
);


-- laba.lab_serv definition

-- Drop table

-- DROP TABLE laba.lab_serv;

CREATE TABLE laba.lab_serv (
	id_lab_ser int4 NOT NULL,
	id_lab_ass int4 NOT NULL,
	id_ser int4 NOT NULL,
	CONSTRAINT lab_serv_pk PRIMARY KEY (id_lab_ser),
	CONSTRAINT lab_serv_fk FOREIGN KEY (id_lab_ass) REFERENCES laba.labarotory_assistant(id_lab_ass),
	CONSTRAINT lab_serv_fk_1 FOREIGN KEY (id_ser) REFERENCES laba.service(id_ser)
);


INSERT INTO laba."role" (id_role,title) VALUES
	 (1,'Lab_ass');

INSERT INTO laba.service (id_ser,title,price,code,term) VALUES
	 (1,'TSH',262.71,619,NULL),
	 (2,'Амилаза',361.88,311,NULL),
	 (3,'Альбумин',234.09,548,NULL),
	 (4,'Креатинин',143.22,258,NULL),
	 (5,'Билирубин общий',102.85,176,NULL),
	 (6,'Гепатит В',176.83,501,NULL),
	 (7,'Гепатит С',289.99,543,NULL),
	 (8,'ВИЧ',490.77,557,NULL),
	 (9,'СПИД',341.78,229,NULL),
	 (10,'Кальций общий',419.9,415,NULL);
INSERT INTO laba.service (id_ser,title,price,code,term) VALUES
	 (11,'Глюкоза',447.65,323,NULL),
	 (12,'Ковид IgM',209.78,855,NULL),
	 (13,'Общий белок',396.03,346,NULL),
	 (14,'Железо',105.32,836,NULL),
	 (15,'Сифилис RPR',443.66,659,NULL),
	 (16,'АТ и АГ к ВИЧ 1/2',370.62,797,NULL),
	 (17,'Волчаночный антикоагулянт',290.11,287,NULL);

INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (2,'Northrop Mably','nmably1','ukM0e6','22.32.15.211','2020-06-20',1),
	 (3,'Fabian Rolf','frolf2','7QpCwac0yi','113.92.142.29','2020-05-19',1),
	 (4,'Lauree Raden','lraden3','5Ydp2mz','39.24.146.52','2020-06-22',1),
	 (5,'Barby Follos','bfollos4','ckmAJPQV','87.232.97.3','2020-03-18',1),
	 (6,'Mile Enterle','menterle5','0PRom6i','85.121.209.6','2020-07-04',1),
	 (7,'Midge Peaker','mpeaker6','0Tc5oRc','196.39.132.128','2020-09-03',1),
	 (8,'Manon Robichon','mrobichon7','LEwEjMlmE5X','143.159.207.105','2020-08-31',1),
	 (9,'Stavro Robken','srobken8','Cbmj3Yi','12.154.73.196','2020-05-22',1),
	 (10,'Bryan Tidmas','btidmas9','dYDHbBQfK','24.42.134.21','2020-06-06',1),
	 (11,'Jeannette Fussie','jfussiea','EGxXuLQ9','98.194.112.137','2020-08-21',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (12,'Stephen Antonacci','santonaccib','YcXAhY3Pja','198.146.255.15','2019-10-03',1),
	 (13,'Niccolo Bountiff','nbountiffc','5xfyjS9ZULGA','231.78.246.229','2020-01-22',1),
	 (14,'Clemente Benjefield','cbenjefieldd','tQOsP0ei9TuD','88.126.93.246','2020-07-09',1),
	 (15,'Orlan Corbyn','ocorbyne','bG1ZIzwIoU','232.175.48.179','2020-04-24',1),
	 (16,'Coreen Stickins','cstickinsf','QRYADbgNj','64.30.175.158','2020-04-20',1),
	 (17,'Daveta Clarage','dclarageg','Yp59ZIDnWe','139.88.229.111','2020-06-09',1),
	 (18,'Javier McCawley','jmccawleyh','g58zLcmCYON','14.199.67.32','2020-04-20',1),
	 (19,'Daile Band','dbandi','yFAaYuVW','206.105.148.56','2019-12-02',1),
	 (20,'Angil Buttery','abutteryj','ttOFbWWGtD','192.158.7.138','2020-06-21',1),
	 (21,'Kyla Kinman','kkinmank','qUr6fdWP6L5G','134.99.243.113','2019-11-08',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (22,'Selena Skepper','sskepperl','jHYN0v3','52.90.89.126','2020-04-28',1),
	 (23,'Alyson Yeoland','ayeolandm','QQezRBV9','239.7.55.187','2020-05-31',1),
	 (24,'Claudie Speeding','cspeedingn','UCLYITfw2Vo','127.37.194.127','2019-11-15',1),
	 (25,'Alaric Scarisbrick','ascarisbricko','fzBcv6GbyCp','97.227.15.172','2020-02-19',1),
	 (26,'Marie Thurby','mthurbyp','wg0uIskqei','94.70.148.135','2019-09-18',1),
	 (27,'Cloe Roxbrough','croxbroughq','67CVVym','185.110.201.36','2020-01-11',1),
	 (28,'Pegeen McCotter','pmccotterr','QG5tdzRpGZJ2','22.179.187.229','2020-03-22',1),
	 (29,'Iggie Calleja','icallejas','aeDvZk8o9','67.237.123.227','2020-07-19',1),
	 (30,'Nelle Brosch','nbroscht','DmPJt2','251.1.59.65','2019-12-17',1),
	 (32,'Eldridge Abbatucci','eabbatucciv','gUtNdsDu','52.44.134.126','2020-03-29',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (33,'Skip Garnham','sgarnhamw','eml6RqbK','100.17.131.54','2020-01-29',1),
	 (34,'Ric Kitchenside','rkitchensidex','xaa7miQ7yB','29.100.76.146','2019-12-14',1),
	 (35,'Urbanus Di Meo','udiy','dHqu78cU6NOP','90.30.202.251','2019-12-25',1),
	 (36,'Monty Beidebeke','mbeidebekez','F5T5spAU9A4O','3.32.202.92','2020-02-05',1),
	 (37,'Byrann Savins','bsavins10','l6sYf29NLN','123.187.14.103','2020-01-23',1),
	 (38,'Sonnie Riby','sriby11','Va34LYqFh','16.81.16.23','2020-06-17',1),
	 (39,'Sherill Birney','sbirney12','Ggygo2ePsETs','144.76.193.237','2019-12-27',1),
	 (41,'Maison Skerme','mskerme14','wy1HWA','143.177.136.232','2020-02-10',1),
	 (42,'Hanan Cahey','hcahey15','NSXcG9khd','18.127.87.158','2020-06-13',1),
	 (43,'Tore Rusling','trusling16','abol9dYC8e','142.216.95.251','2020-03-19',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (44,'Jeddy De Souza','jde17','gK6Hsl8Q','229.104.255.175','2019-10-17',1),
	 (45,'Flossi McLeoid','fmcleoid18','B9zr0N7cJw','90.207.38.179','2020-01-26',1),
	 (46,'Nikoletta Megainey','nmegainey19','gph7QurFf','172.249.218.50','2020-05-22',1),
	 (47,'Adan Bliven','abliven1a','vVxlf94KpeX','49.101.94.118','2020-06-17',1),
	 (48,'Mohandis Rossoni','mrossoni1b','nLXj2lS','161.5.132.42','2019-11-16',1),
	 (49,'Nappie Redington','nredington1c','DCbOb1SX','174.42.8.3','2020-05-06',1),
	 (50,'Lenka Francie','lfrancie1d','DoGeHWuAAM','182.2.128.34','2020-03-30',1),
	 (51,'Ashley Blowin','ablowin1e','aQygVtMjN','73.212.243.168','2020-06-24',1),
	 (52,'Vale Goroni','vgoroni1f','bWr0QU','93.126.120.134','2020-08-19',1),
	 (53,'Suki Grafom','sgrafom1g','JcNcVDAouYzA','9.26.5.107','2019-12-17',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (55,'Emilie Collett','ecollett1i','Y0uMyKB0W','47.0.240.7','2019-10-07',1),
	 (56,'Byrom Terrell','bterrell1j','hswseW','157.21.33.53','2020-02-25',1),
	 (57,'Daphne Bifield','dbifield1k','oYAQ4URihIA','24.185.229.169','2020-07-29',1),
	 (58,'Blanca Staig','bstaig1l','MygqEtjMtUbC','171.78.28.229','2020-02-19',1),
	 (59,'Adriaens Kennsley','akennsley1m','CTUdBfJsy6qF','208.81.128.179','2020-07-15',1),
	 (60,'Emlyn Bartak','ebartak1n','y3t4H1','130.247.20.138','2019-12-20',1),
	 (61,'Victoria Willshire','vwillshire1o','VFSLc2t','243.230.165.161','2020-09-03',1),
	 (62,'Egon Savin','esavin1p','axnJY9s','40.140.160.210','2020-01-31',1),
	 (63,'Phillie Elsom','pelsom1q','OXzMECG','253.7.8.82','2020-01-01',1),
	 (64,'Adan Semaine','asemaine1r','MdJRkHor5SP','76.252.15.218','2019-10-05',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (65,'Constantino Northrop','cnorthrop1s','UIwCvTA7MRS0','119.130.24.85','2019-10-12',1),
	 (66,'Rodie Easson','reasson1t','3J0jgg9RWlXs','212.248.119.232','2020-08-14',1),
	 (67,'Alida Boleyn','aboleyn1u','3q2mQdDRmtr','181.14.56.184','2020-05-26',1),
	 (68,'Hill Scholfield','hscholfield1v','1Pbs3K6qXYB','15.7.205.224','2020-02-23',1),
	 (70,'Alexandro Eldon','aeldon1x','rrywOQRmFKyh','4.174.11.210','2019-12-04',1),
	 (71,'Kayle Collin','kcollin1y','Q0ZV21vew0','52.19.142.168','2020-06-30',1),
	 (72,'Inesita Larkins','ilarkins1z','DEFNpHtU','3.26.42.188','2019-12-05',1),
	 (73,'Waylin Lound','wlound20','a2G4Ihh2o','31.243.68.215','2020-01-26',1),
	 (74,'Mechelle Gillogley','mgillogley21','EjUHfCUFqF','79.38.53.53','2020-05-01',1),
	 (75,'Donal Muccino','dmuccino22','E4okVgx','109.138.101.234','2020-04-02',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (76,'Joye Leadbetter','jleadbetter23','ZNsaKdgb','51.245.190.167','2020-05-02',1),
	 (77,'Gianina Trump','gtrump24','6XXY7IS26Ci','11.191.37.17','2020-08-03',1),
	 (78,'Read LeEstut','rleestut25','zq3C4rUR','119.247.100.162','2020-09-11',1),
	 (79,'Jill Anscott','janscott26','5maCRrCZLu','104.85.178.46','2020-04-28',1),
	 (80,'Bud Douch','bdouch27','KAkwrli','72.132.101.188','2020-02-06',1),
	 (82,'Hew Izzat','hizzat29','Uifdtu','143.246.125.169','2020-01-20',1),
	 (1,'Clareta Hacking','chacking0','4tzqHdkqzo4','147.231.50.234','2020-02-10',1),
	 (31,'Shae Allsepp','sallseppu','t0ko0854Cpvv','88.20.74.85','2020-08-09',1),
	 (40,'Indira Kleanthous','ikleanthous13','3H0GS7a','169.108.108.88','2019-12-29',1),
	 (54,'Justis Gianneschi','jgianneschi1h','oieX5u3sUfpD','139.241.156.87','2020-03-14',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (69,'Cordell Cowpe','ccowpe1w','VHr417Ft0','237.236.173.63','2020-06-17',1),
	 (81,'Cicily Ossenna','cossenna28','vfKJkCeohOzZ','230.85.180.186','2020-01-06',1),
	 (83,'Eddie Gimeno','egimeno2a','oF1hbmKlZ','60.57.115.125','2020-03-18',1),
	 (84,'Sybyl Fierro','sfierro2b','VjUrQ2','250.233.247.215','2020-01-22',1),
	 (85,'Nicol Troup','ntroup2c','KmDDYf1Pu','121.7.142.165','2020-06-25',1),
	 (86,'Bondy Pattenden','bpattenden2d','IOUkHpOn','45.121.26.90','2020-06-15',1),
	 (87,'Angus Cockman','acockman2e','fDKhK7OK','167.9.255.77','2020-01-06',1),
	 (88,'Mord Hanscome','mhanscome2f','xBHzpa7eP0u','121.181.10.230','2020-07-10',1),
	 (89,'Susy Leblanc','sleblanc2g','T2et1U5M','118.164.120.202','2020-06-16',1),
	 (90,'Gerard Ciccoloi','gciccoloi2h','w4dZ3hxiCiAg','71.235.27.27','2020-02-03',1);
INSERT INTO laba."user" (id_user,"name",login,"password",ip,lastenter,id_role) VALUES
	 (91,'Seamus Sayburn','ssayburn2i','1hTM7EVKaS','75.194.92.114','2020-01-24',1),
	 (92,'Washington Gentiry','wgentiry2j','z2X9UH5','188.49.78.185','2020-04-10',1),
	 (93,'Rebekkah Westall','rwestall2k','xLgunbO9x6','212.150.81.93','2020-02-02',1),
	 (94,'Court Kulic','ckulic2l','FLHYRN','154.121.193.131','2020-06-26',1),
	 (95,'Lorilee Roux','lroux2m','98cCxHeeK31','229.187.60.106','2020-06-12',1),
	 (96,'Modestine Rolinson','mrolinson2n','faGzyW8hEca','9.203.185.188','2019-10-30',1),
	 (97,'Shelbi Ellgood','sellgood2o','3do5MME','199.226.26.7','2020-08-31',1),
	 (98,'Barbabra Retchless','bretchless2p','WraGihh','86.66.23.203','2019-11-09',1),
	 (99,'Robinetta Jerzak','rjerzak2q','hAp8jki','205.158.144.210','2019-12-11',1),
	 (100,'Vance Boots','vboots2r','bgJfSDEVEQm6','91.73.40.29','2020-09-07',1);


INSERT INTO laba.type_lab (id_type_lab,"type") VALUES
	 (1,1),
	 (2,2),
	 (3,3);

INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (1,1,1),
	 (2,2,2),
	 (3,3,1),
	 (4,4,1),
	 (5,5,1),
	 (6,6,2),
	 (7,7,2),
	 (8,8,2),
	 (9,9,3),
	 (10,10,3);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (11,11,2),
	 (12,12,2),
	 (13,13,1),
	 (14,14,3),
	 (15,15,3),
	 (16,16,1),
	 (17,17,3),
	 (18,18,3),
	 (19,19,3),
	 (20,20,2);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (21,21,1),
	 (22,22,3),
	 (23,23,3),
	 (24,24,2),
	 (25,25,3),
	 (26,26,1),
	 (27,27,2),
	 (28,28,3),
	 (29,29,2),
	 (30,30,3);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (31,31,2),
	 (32,32,2),
	 (33,33,2),
	 (34,34,2),
	 (35,35,2),
	 (36,36,2),
	 (37,37,2),
	 (38,38,2),
	 (39,39,1),
	 (40,40,2);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (41,41,2),
	 (42,42,3),
	 (43,43,2),
	 (44,44,2),
	 (45,45,2),
	 (46,46,2),
	 (47,47,3),
	 (48,48,3),
	 (49,49,2),
	 (50,50,2);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (51,51,2),
	 (52,52,3),
	 (53,53,3),
	 (54,54,3),
	 (55,55,2),
	 (56,56,1),
	 (57,57,1),
	 (58,58,3),
	 (59,59,3),
	 (60,60,1);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (61,61,1),
	 (62,62,2),
	 (63,63,2),
	 (64,64,2),
	 (65,65,1),
	 (66,66,2),
	 (67,67,3),
	 (68,68,2),
	 (69,69,3),
	 (70,70,1);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (71,71,1),
	 (72,72,1),
	 (73,73,1),
	 (74,74,2),
	 (75,75,3),
	 (76,76,2),
	 (77,77,3),
	 (78,78,1),
	 (79,79,3),
	 (80,80,1);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (81,81,3),
	 (82,82,1),
	 (83,83,3),
	 (84,84,2),
	 (85,85,2),
	 (86,86,1),
	 (87,87,1),
	 (88,88,2),
	 (89,89,2),
	 (90,90,1);
INSERT INTO laba.labarotory_assistant (id_lab_ass,id_user,id_type_lab) VALUES
	 (91,91,2),
	 (92,92,1),
	 (93,93,2),
	 (94,94,1),
	 (95,95,3),
	 (96,96,1),
	 (97,97,3),
	 (98,98,2),
	 (99,99,2),
	 (100,100,2);

INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (1,1,8),
	 (2,1,14),
	 (3,1,17),
	 (4,2,12),
	 (5,2,1),
	 (6,2,8),
	 (7,2,14),
	 (8,2,3),
	 (9,3,7),
	 (10,3,14);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (11,4,12),
	 (12,4,4),
	 (13,5,7),
	 (14,5,10),
	 (15,5,1),
	 (16,5,8),
	 (17,6,8),
	 (18,6,14),
	 (19,6,9),
	 (20,7,17);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (21,7,1),
	 (22,7,3),
	 (23,7,13),
	 (24,8,17),
	 (25,8,1),
	 (26,8,3),
	 (27,8,13),
	 (28,9,11),
	 (29,9,3),
	 (30,9,13);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (31,10,9),
	 (32,10,13),
	 (33,10,6),
	 (34,10,3),
	 (35,11,17),
	 (36,12,1),
	 (37,12,4),
	 (38,12,9),
	 (39,12,8),
	 (40,12,16);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (41,13,11),
	 (42,13,2),
	 (43,13,8),
	 (44,14,12),
	 (45,14,5),
	 (46,14,12),
	 (47,15,9),
	 (48,15,14),
	 (49,15,17),
	 (50,15,1);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (51,16,8),
	 (52,17,13),
	 (53,17,14),
	 (54,18,1),
	 (55,18,4),
	 (56,19,5),
	 (57,19,7),
	 (58,19,12),
	 (59,19,4),
	 (60,20,11);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (61,20,12),
	 (62,20,15),
	 (63,20,17),
	 (64,21,13),
	 (65,21,12),
	 (66,21,11),
	 (67,21,8),
	 (68,21,6),
	 (69,22,12),
	 (70,22,17);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (71,23,14),
	 (72,23,9),
	 (73,23,4),
	 (74,23,14),
	 (75,24,3),
	 (76,24,8),
	 (77,24,5),
	 (78,25,7),
	 (79,25,14),
	 (80,26,3);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (81,26,12),
	 (82,27,6),
	 (83,27,16),
	 (84,28,5),
	 (85,28,12),
	 (86,29,14),
	 (87,29,5),
	 (88,29,1),
	 (89,29,4),
	 (90,30,13);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (91,31,3),
	 (92,31,9),
	 (93,31,4),
	 (94,31,1),
	 (95,32,14),
	 (96,32,16),
	 (97,33,3),
	 (98,34,6),
	 (99,34,4),
	 (100,34,15);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (101,34,15),
	 (102,35,16),
	 (103,35,17),
	 (104,35,12),
	 (105,35,13),
	 (106,36,10),
	 (107,36,7),
	 (108,36,14),
	 (109,37,14),
	 (110,38,6);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (111,39,14),
	 (112,39,4),
	 (113,39,1),
	 (114,40,16),
	 (115,40,13),
	 (116,40,7),
	 (117,40,6),
	 (118,40,4),
	 (119,41,5),
	 (120,41,1);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (121,42,6),
	 (122,42,4),
	 (123,43,1),
	 (124,44,10),
	 (125,44,1),
	 (126,45,7),
	 (127,46,10),
	 (128,47,11),
	 (129,47,3),
	 (130,48,5);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (131,48,15),
	 (132,48,17),
	 (133,49,3),
	 (134,50,15),
	 (135,50,6),
	 (136,50,10),
	 (137,51,2),
	 (138,51,12),
	 (139,51,5),
	 (140,52,17);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (141,52,15),
	 (142,53,16),
	 (143,53,11),
	 (144,53,3),
	 (145,54,14),
	 (146,54,7),
	 (147,54,3),
	 (148,54,2),
	 (149,55,7),
	 (150,55,17);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (151,55,4),
	 (152,56,17),
	 (153,56,8),
	 (154,56,7),
	 (155,57,1),
	 (156,57,11),
	 (157,57,12),
	 (158,57,9),
	 (159,58,5),
	 (160,58,12);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (161,59,14),
	 (162,59,3),
	 (163,60,7),
	 (164,61,8),
	 (165,61,17),
	 (166,61,14),
	 (167,62,13),
	 (168,62,4),
	 (169,62,7),
	 (170,62,11);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (171,62,5),
	 (172,63,11),
	 (173,63,6),
	 (174,64,4),
	 (175,65,5),
	 (176,65,9),
	 (177,66,12),
	 (178,66,17),
	 (179,66,16),
	 (180,67,9);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (181,67,11),
	 (182,67,6),
	 (183,67,4),
	 (184,67,10),
	 (185,68,1),
	 (186,68,4),
	 (187,68,6),
	 (188,68,17),
	 (189,69,13),
	 (190,69,2);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (191,69,13),
	 (192,70,14),
	 (193,70,13),
	 (194,71,13),
	 (195,72,3),
	 (196,72,4),
	 (197,72,2),
	 (198,72,9),
	 (199,73,1),
	 (200,73,15);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (201,73,13),
	 (202,74,13),
	 (203,74,5),
	 (204,74,5),
	 (205,74,10),
	 (206,75,5),
	 (207,75,1),
	 (208,75,17),
	 (209,75,2),
	 (210,76,7);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (211,76,3),
	 (212,76,12),
	 (213,77,4),
	 (214,78,11),
	 (215,78,17),
	 (216,78,15),
	 (217,78,5),
	 (218,79,17),
	 (219,80,17),
	 (220,81,6);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (221,81,5),
	 (222,82,6),
	 (223,82,17),
	 (224,82,2),
	 (225,83,15),
	 (226,83,7),
	 (227,83,6),
	 (228,83,15),
	 (229,83,3),
	 (230,84,10);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (231,84,11),
	 (232,84,13),
	 (233,84,2),
	 (234,85,10),
	 (235,85,5),
	 (236,85,14),
	 (237,85,17),
	 (238,86,14),
	 (239,86,9),
	 (240,86,13);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (241,87,11),
	 (242,87,9),
	 (243,87,4),
	 (244,87,10),
	 (245,88,13),
	 (246,88,3),
	 (247,89,12),
	 (248,89,1),
	 (249,90,3),
	 (250,90,2);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (251,90,13),
	 (252,91,8),
	 (253,91,3),
	 (254,91,11),
	 (255,91,8),
	 (256,92,11),
	 (257,92,12),
	 (258,92,9),
	 (259,93,6),
	 (260,93,12);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (261,93,16),
	 (262,94,11),
	 (263,95,10),
	 (264,95,7),
	 (265,96,2),
	 (266,96,12),
	 (267,96,16),
	 (268,97,5),
	 (269,97,14),
	 (270,97,13);
INSERT INTO laba.lab_serv (id_lab_ser,id_lab_ass,id_ser) VALUES
	 (271,97,6),
	 (272,97,5),
	 (273,98,9),
	 (274,98,10),
	 (275,98,4),
	 (276,98,1),
	 (277,99,15),
	 (278,100,3),
	 (279,100,13),
	 (280,100,2);






