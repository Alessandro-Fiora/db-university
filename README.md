# ESERCIZIO STRUTTURA DATABASE UNIVERSITA'

Ho avuto un dubbio durante lo svolgimento: non è specificato che un teacher possa avere più corsi, nel caso in cui potesse, la relazione tra corso e insegnante sarebbe many to many, e si aggiungerebbe una tabella pivot (subject_teacher) con subject_id e teacher_id come campi

# ESERCIZIO 07/01/25

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

# ESERCIZIO 08/01/25

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT `students`.\*,
`degrees`.`name`
FROM `university`.`students`
JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`id` = 53;

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze

SELECT
`courses`.\*,
`degrees`.`level`,
`departments`.`name` AS `department_name`
FROM `university`.`courses`
JOIN `degrees`
ON `courses`.`degree_id` = `degrees`.`id`

JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`

WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name` = 'Dipartimento di Neuroscienze';

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

SELECT `courses`.\*
FROM `university`.`course_teacher`
JOIN `courses`
ON `course_teacher`.`course_id` = `courses`.`id`
WHERE `teacher_id` = 44;

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome

SELECT \*
FROM `university`.`students`

JOIN `degrees`
ON `students`.`degree_id` = `degrees`.`id`

JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`

ORDER BY `students`.`name`, `students`.`surname`
;

5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT
`degrees`.`name` AS `degree_name`,
`courses`.`name` AS `course_name`,
`teachers`.`name` AS `teacher_name`,
`teachers`.`surname` AS `teacher_surname`

FROM university.degrees
JOIN `courses`
ON `degrees`.`id`=`courses`.`degree_id`

JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`
;

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica (54)

SELECT `teachers`.\*
FROM `university`.`departments`

JOIN `degrees`
ON `departments`.`id`=`degrees`.`department_id`

JOIN `courses`
ON `degrees`.`id`=`courses`.`degree_id`

JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`

JOIN `teachers`
ON `course_teacher`.`teacher_id` = `teachers`.`id`

WHERE `departments`.`name` = 'Dipartimento di Matematica'

GROUP BY `teachers`.`id`;

7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18
