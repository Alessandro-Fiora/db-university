# ESERCIZIO STRUTTURA DATABASE UNIVERSITA'

Ho avuto un dubbio durante lo svolgimento: non è specificato che un teacher possa avere più corsi, nel caso in cui potesse, la relazione tra corso e insegnante sarebbe many to many, e si aggiungerebbe una tabella pivot (subject_teacher) con subject_id e teacher_id come campi

# ESERCIZIO MYSQL

1. Selezionare tutti gli studenti nati nel 1990 (160)

SELECT \*
FROM `university`.`students`
WHERE YEAR(`date_of_birth`) = 1990;

2. Selezionare tutti i corsi che valgono più di 10 crediti (479)

SELECT \* FROM `university`.`courses`
WHERE `cfu` > 10;

3. Selezionare tutti gli studenti che hanno più di 30 anni

SELECT \* FROM `university`.`students`
WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURRENT_DATE()) > 30;

4. Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
   laurea (286)

SELECT \* FROM `university`.`courses`
WHERE `year` = 1 AND `period` LIKE 'I sem%';

5. Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
   20/06/2020 (21)

SELECT \* FROM `university`.`exams`
WHERE `date` = '2020-06-20' AND `hour` > '14:00:00';

6. Selezionare tutti i corsi di laurea magistrale (38)

SELECT \* FROM `university`.`degrees`
WHERE `level` = 'magistrale';

7. Da quanti dipartimenti è composta l'università? (12)

SELECT COUNT(`id`) FROM `university`.`departments`;

8. Quanti sono gli insegnanti che non hanno un numero di telefono? (50)

SELECT COUNT(`id`) FROM `university`.`teachers`
WHERE `phone` IS NULL;

9. Inserire nella tabella degli studenti un nuovo record con i propri dati (per il campo
   degree_id, inserire un valore casuale)

INSERT INTO `university`.`students` (`degree_id`, `name`, `surname`, `date_of_birth`, `fiscal_code`, `enrolment_date`, `registration_number`, `email`) VALUES ('2', 'Alessandro', 'Fiora', '1997-04-03', 'FRINLD95O56I599Q', '2018-06-10', '621033', 'ciao.ciao@ciao.com');

10. Cambiare il numero dell’ufficio del professor Pietro Rizzo in 126

UPDATE `university`.`teachers` SET `office_number` = '126' WHERE (`id` = 58);

11. Eliminare dalla tabella studenti il record creato precedentemente al punto

DELETE FROM `university`.`students` WHERE (`id` = 5014);

# BONUS GROUP BY

1. Contare quanti iscritti ci sono stati ogni anno

2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
3. Calcolare la media dei voti di ogni appello d'esame
4. Contare quanti corsi di laurea ci sono per ogni dipartiment
