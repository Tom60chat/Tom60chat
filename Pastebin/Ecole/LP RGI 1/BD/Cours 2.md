# Cours 2 - SQL avancé 

## Prérequis

Une base de donnée : Oracle 21c
Et le logiciel SQL Developer

## II - Contraintes

1. Créez une table contenant les étudiants de la promotion avec :
- Etudiant_id : clé primaire unique not null
- Nom : non null pas plus de 100 caractères
- Prénom : non null pas plus de 60 caractères
- Date de naissance : après 1880

```sql
CREATE TABLE etudiant (
    etudiant_id NUMBER PRIMARY KEY,
    nom VARCHAR2(100) NOT NULL,
    prenom VARCHAR2(60) NOT NULL,
    date_naissance DATE CHECK (date_naissance > DATE '1880-12-31')
);
```

Tests des contraintes :
    
```sql
-- Doit échouer
INSERT INTO etudiant VALUES (1, null, null, null);
INSERT INTO etudiant VALUES (5, 'primary', 'key', DATE '1880-12-31');
INSERT INTO etudiant VALUES (5, 'primary', 'key2', DATE '1880-12-31');
# INSERT INTO etudiant VALUES (2, 'Non in enim sunt aute enim commodo Laborum consequat non sit occaecat in eu est duis eiusmod ullamco+', 'Bar', TO_DATE('01/01/2000', 'DD/MM/YYYY'));
INSERT INTO etudiant VALUES (3, 'Void', 'Non in enim sunt aute enim commodo Laborum consequat non sit+', TO_DATE('01/01/2000', 'DD/MM/YYYY'));
INSERT INTO etudiant VALUES (4, 'Test', 'Test', DATE '1880-12-31');

-- Doit réussir
INSERT INTO etudiant VALUES (1, 'Tom', 'Lechat', DATE '1881-01-01');
```

2. Créez une table contenant les formations universitaires existantes (DUT info, licence RGI, bac, etc.)
- Formation_ID : primaire unique not null. Indication : la contrainte de clé primaire aura un nom que vous définirez
- Intitulé formation
- Niveau : nombre de 0 à 8 pour nombre d’année après le BAC (0 = niveau BAC, 1 = BAC+1, etc.)
- Niveau européen : Licence, Master, Doctorat.

```sql
CREATE TABLE formation (
    formation_id NUMBER PRIMARY KEY,
    intitule VARCHAR2(100),
    niveau INT CHECK (niveau BETWEEN 0 AND 8),
    niveau_europeen VARCHAR2(100) CHECK (niveau_europeen IN ('Licence', 'Master', 'Doctorat'))
);
```

Tests :
    
```sql
-- Doit échouer
INSERT INTO formation VALUES (1, 'Do elit ipsum proident qui nisi aliqua proident cillum est ex est Ea mollit proident sunt nostrud dolor do eiusmod laboris ipsum cupidatat dolore', 2, 'Licence');
INSERT INTO formation VALUES (1, 'Do elit', 9, 'Licence');
INSERT INTO formation VALUES (1, 'Do elit', 3, 'Catmaster');

INSERT INTO formation VALUES (1, 'Key', 1, 'Licence');
INSERT INTO formation VALUES (1, 'Key2', 1, 'Licence');

-- Doit réussir
INSERT INTO formation VALUES (1, 'DUT info', 8, 'Licence');
```

3. Créez une table liant les 2 tables (un étudiant peut avoir plusieurs formations et une formation, plusieurs étudiants).
- Etudiant_formation_ID : primaire non null, vous choisirez l’index de clé primaire créé automatiquement
- Etudiant_id : clé étrangère sur étudiant
- Formation_id : clé étrangère sur formation
- Date de début
- Date de fin : la date de fin doit être postérieure à la date de début
- Obtenu : contient seulement Y ou N
- Mention : contient seulement P, AB, B, TB

```sql
CREATE TABLE etudiant_formation (
    etudiant_formation_id NUMBER PRIMARY KEY,
    etudiant_id NUMBER REFERENCES etudiant(etudiant_id),
    formation_id NUMBER REFERENCES formation(formation_id),
    date_debut DATE,
    date_fin DATE,
    obtenu CHAR(1) CHECK (obtenu IN ('Y', 'N')),
    mention CHAR(2) CHECK (mention IN ('P', 'AB', 'B', 'TB')),

    CONSTRAINT date_check CHECK (date_fin > date_debut)
);
```

Tests :
    
```sql
-- Doit échouer
INSERT INTO etudiant_formation VALUES (1, 1, 1, DATE '1881-12-31', DATE '1880-12-31', 'X', 'X');

-- Doit réussir
INSERT INTO etudiant_formation VALUES (1, 1, 1, DATE '2022-01-01', DATE '2023-01-01', 'Y', 'P');
```


4. Retrouvez vos tables, contraintes, index et autres dans les tables all_*

```sql
SELECT * FROM all_tables;
SELECT * FROM all_indexes;
SELECT * FROM all_constraints;
```

## III - Les vues

1. Créez une vue affichant les employee_id et leur job actuel (table EMPLOYEES uniquement)
- Faites des UPDATE sur cette vue : que constatez-vous ?
- Vérifiez le résultat dans la table
- Expliquez-moi l’utilité de pouvoir faire cela ? Parce qu’à priori, si l’on crée une vue sur une
seule table et qu’on peut la mettre à jour, pourquoi ne pas utiliser directement la table … ?

```sql
CREATE VIEW employee_job AS
SELECT employee_id, job_id
FROM employees;

UPDATE employee_job SET job_id = 'IT_PROG' WHERE employee_id = 100;
```

On peut constater que la vue est mise à jour mais pas la table.

L'utilité de pouvoir faire cela est de pouvoir mettre à jour une vue qui est composée de plusieurs tables.


2. Faite de même mais en allant chercher leur dernier précédent job dans la table JOB_HISTORY
- Attention, les employés qui n’ont eu qu’un seul job n’ont pas d’historique, il faut qu’ils apparaissent quand même lorsque l’on interroge la vue
- Faites des UPDATE sur cette vue : que constatez-vous ?

```sql
CREATE VIEW employee_job_history AS
SELECT e.employee_id, e.job_id, jh.start_date, jh.end_date
FROM employees e
LEFT JOIN job_history jh ON e.employee_id = jh.employee_id;

UPDATE employee_job_history SET job_id = 'IT_PROG' WHERE employee_id = 100;
```

On peut constater que la vue est mise à jour mais pas la table.

3. Créez une vue affichant chaque département (table DEPARTMENTS) ainsi que le salaire moyen,
maximum et minimum (MIN et MAX de la colonne SALARY de la table EMPLOYEES de ce
département).

```sql
CREATE VIEW department_salary AS
SELECT d.department_id, d.department_name, AVG(e.salary) AS avg_salary, MIN(e.salary) AS min_salary, MAX(e.salary) AS max_salary
FROM departments d
LEFT JOIN employees e ON d.department_id = e.department_id
GROUP BY d.department_id, d.department_name;
```

4. Créez une vue matérialisée contenant la liste des employés ainsi que leur département d’appartenance.
- Consultez cette vue.
- Insérer un nouvel employé dans la table.
- Interroger à nouveau la vue : que constatez-vous ?
- Rafraichissez la vue et interroger là à nouveau pour vérifier la présence de ce nouvel employé.

```sql
CREATE MATERIALIZED VIEW employee_department AS
SELECT e.employee_id, e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;

SELECT * FROM employee_department;

INSERT INTO employees VALUES (207, 'Test', 'Test', 'TEST', '123.456.789.123', DATE '1880-12-31', 'IT_PROG', 100, 100, 10);

SELECT * FROM employee_department;

EXEC DBMS_MVIEW.REFRESH('EMPLOYEE_DEPARTMENT');

SELECT * FROM employee_department;
```

## IV - Large Objects (LOB)

1. Essayez de créer une table avec plusieurs champs LONG

```sql
CREATE TABLE test_long (
    id NUMBER PRIMARY KEY,
    long1 LONG,
    long2 LONG
);
```

ça ne fonctionne pas.

2. Ajoutez 2 CLOB dans la table des étudiants : le premier nommé « Biographie » et le 2ème nommé « notes »
- Insérez-y des valeurs

```sql
ALTER TABLE etudiant ADD (biographie CLOB, notes CLOB);

UPDATE etudiant SET biographie = 'Lorem ipsum dolor sit amet.' WHERE etudiant_id = 1;
UPDATE etudiant SET notes = 'Consectetur adipiscing elit.' WHERE etudiant_id = 1;
```

## V - Générer du XML en SQL


