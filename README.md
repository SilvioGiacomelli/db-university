# db-university

1
Selezionare tutti gli studenti nati nel 1990 (160)
SELECT `date_of_birth`
FROM `students`
WHERE YEAR(`date_of_birth`) = 1990;

2
Selezionare tutti i corsi che valgono più di 10 crediti (479)
SELECT \*
FROM `courses`
WHERE `cfu` > 10

3
Selezionare tutti gli studenti che hanno più di 30 anni
SELECT \*
FROM `students`
WHERE TIMESTAMPDIFF(YEAR,`date_of_birth`, CURDATE())>30

4
Selezionare tutti i corsi del primo semestre del primo anno di un qualsiasi corso di
laurea
SELECT \*
FROM `courses`
WHERE period = 'I semestre' AND year = 1;

5
Selezionare tutti gli appelli d'esame che avvengono nel pomeriggio (dopo le 14) del
20/06/2020
SELECT \*
FROM `exams`
WHERE date = '2020-06-20' AND hour >= '14:00:00';

6
Selezionare tutti i corsi di laurea magistrale
SELECT \*
FROM `degrees`
WHERE level = 'magistrale';

7
Da quanti dipartimenti è composta l'università?
SELECT COUNT(\*)
FROM `departments`

8 Quanti sono gli insegnanti che non hanno un numero di telefono?
SELECT COUNT(\*)
FROM `teachers`
WHERE `phone` IS NULL

/////////////////////////

1 Contare quanti iscritti ci sono stati ogni anno
SELECT COUNT(\*), YEAR(`enrolment_date`)
FROM `students`
GROUP BY YEAR(`enrolment_date`)

2 Contare gli insegnanti che hanno l'ufficio nello stesso edificio
SELECT `office_address`,COUNT(\*) as `numero_insegnanti`
FROM `teachers`
GROUP BY `office_address`

3 Calcolare la media dei voti di ogni appello d'esame
SELECT `exam_id`, AVG(`vote`) AS `media_voto`
FROM `exam_student`
GROUP BY `exam_id`

4 Contare quanti corsi di laurea ci sono per ogni dipartimento
SELECT `department_id`, COUNT(`id`) AS `numero_corsi`
FROM `degrees`
GROUP BY `department_id`

/////////////////////////

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
   SELECT `students`.`name`,`students`.`surname`
   FROM `degrees`
   INNER JOIN `students` ON `degrees`.`id` = `students`.`degree_id`
   WHERE `degrees`.`name` = 'Corso di Laurea in Economia'

2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze
   SELECT `degrees`.`name`
   FROM `departments`
   INNER JOIN `degrees` ON `departments`.`id` = `degrees`.`department_id`
   WHERE `degrees`.`level` = 'magistrale' AND `departments`.`name` = 'Dipartimento di Neuroscienze'

3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
   SELECT `courses`.`name`, `teachers`.`name`, `teachers`.`surname`
   FROM `teachers`
   INNER JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
   INNER JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
   WHERE `teachers`.`name` = 'Fulvio' AND `teachers`.`surname` = 'Amato'

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome
   SELECT `students`.`name`, `students`.`surname`, `departments`.`name`, `degrees`.`name`
   FROM `students`
   INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
   INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
   ORDER BY students.surname ASC
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
   SELECT `teachers`.`name`, `teachers`.`surname`, `degrees`.`name`, `course`.`name`
   FROM `degrees`
   INNER JOIN `courses` ON `degrees`.`id` = `courses`.`degree_id`
   INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
   INNER JOIN `teachers` on `course_teacher`.`teacher_id` = `teachers`.`id`

6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica
   SELECT `teachers`.`name`, `teachers`.`surname`
   FROM `teachers`
   INNER JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
   INNER JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
   INNER JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
   INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
   WHERE `departments`.`name` = 'Dipartimento di Matematica'
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18
   /////////////////////////////////////////////////////////////////////
