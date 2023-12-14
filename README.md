### RESULTADOS 

1. Devuelve un listado con el primer apellido, segundo apellido y el nombre de todos los alumnos. El listado deberá estar ordenado alfabéticamente de menor a mayor por el primer apellido, segundo apellido y nombre.

    ```sql
    DESCRIBE persona;

    SELECT CONCAT(apellido1 ," " ,apellido2 , " " ,nombre)
    from persona
    WHERE tipo= 'alumno'
    ORDER BY (apellido1) ASC,(apellido2) ASC,(nombre) ASC;
    ```

2. Averigua el nombre y los dos apellidos de los alumnos que **no** han dado de alta su número de teléfono en la base de datos.

    ```sql
    SELECT *
    from persona;

    SELECT CONCAT(apellido1 ," " ,apellido2 , " " ,nombre),telefono
    from persona
    WHERE telefono is NULL
    AND tipo= 'alumno';
    ```

3. Devuelve el listado de los alumnos que nacieron en `1999`.

    ```sql
    SELECT *
    from persona;

    SELECT nombre,fecha_nacimiento 
    from persona
    WHERE fecha_nacimiento LIKE '1999%'
    AND tipo= 'alumno';
    ```

4. Devuelve el listado de `profesores` que **no** han dado de alta su número de teléfono en la base de datos y además su nif termina en `K`.

    ```sql
    DESCRIBE profesor;

    SELECT p.nombre,p.telefono,p.nif
    FROM persona p
    JOIN profesor pro on p.id=pro.id_profesor
    WHERE p.telefono is NULL
    AND p.nif LIKE '%k'
    AND tipo= 'profesor';
    ```

5. Devuelve el listado de las asignaturas que se imparten en el primer cuatrimestre, en el tercer curso del grado que tiene el identificador `7`.

    ```sql
    DESCRIBE asignatura;

    SELECT *
    FROM asignatura;

    SELECT nombre,cuatrimestre,curso,id_grado as 'id del grado'
    FROM asignatura
    WHERE cuatrimestre=1
    AND curso=3
    AND id_grado=7;
    ```

6. Devuelve un listado con los datos de todas las **alumnas** que se han matriculado alguna vez en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    SELECT *
    FROM alumno_se_matricula_asignatura;

    SELECT *
    FROM grado
    WHERE grado.nombre LIKE '%Ingeniería%';

    SELECT DISTINCT p.nombre,p.apellido1,p.apellido2,g.nombre
    FROM alumno_se_matricula_asignatura asma
    JOIN persona p on p.id=asma.id_alumno
    JOIN asignatura a on a.id=asma.id_asignatura
    JOIN grado g on g.id=a.id_grado
    WHERE p.sexo= 'M'
    AND g.id= '4';
    ```

7. Devuelve un listado con todas las asignaturas ofertadas en el `Grado en Ingeniería Informática (Plan 2015)`.

    ```sql
    SELECT DISTINCT g.nombre as GRADO,a.nombre as MATERIA
    FROM grado g
    JOIN asignatura a on g.id=a.id_grado
    WHERE g.id = 4; 
    ```

8. Devuelve un listado de los `profesores` junto con el nombre del `departamento` al que están vinculados. El listado debe devolver cuatro columnas, `primer apellido, segundo apellido, nombre y nombre del departamento.` El resultado estará ordenado alfabéticamente de menor a mayor por los `apellidos y el nombre.`

    ```sql
    SELECT DISTINCT pe.apellido1,pe.apellido2,pe.nombre,d.nombre as 'NOMBRE DEPARTAMENTO'
    FROM profesor p
    JOIN departamento d on d.id=p.id_departamento
    JOIN persona pe on pe.id=p.id_profesor
    ORDER BY (apellido1) ASC,(apellido2) ASC,(nombre) ASC;
    ```

9. Devuelve un listado con el nombre de las asignaturas, año de inicio y año de fin del curso escolar del alumno con nif `26902806M`.

    ```sql
    SELECT DISTINCT a.nombre as 'asignatura',ce.anyo_inicio as 'año inicial',
    ce.anyo_fin as 'año final',p.nif
    FROM alumno_se_matricula_asignatura asma
    JOIN asignatura a on a.id=asma.id_asignatura
    JOIN persona p on p.id=asma.id_alumno
    JOIN curso_escolar ce on ce.id=asma.id_curso_escolar
    WHERE p.nif='26902806M';
    ```

10. Devuelve un listado con el nombre de todos los departamentos que tienen profesores que imparten alguna asignatura en el `Grado en Ingeniería Informática (Plan 2015)`.

     ```sql
    SELECT DISTINCT d.nombre from asignatura a
    JOIN profesor p on a.id = p.id_profesor
    JOIN grado g on a.id_grado = g.id
    JOIN departamento d on p.id_departamento = d.id
    WHERE g.id = 4
     ```

11. Devuelve un listado con todos los alumnos que se han matriculado en alguna asignatura durante el curso escolar 2018/2019.

     ```sql
    SELECT DISTINCT p.nombre,ce.anyo_fin as 'años entre 2018 y 2019'
    FROM alumno_se_matricula_asignatura asma
    JOIN persona p on p.id=asma.id_alumno
    JOIN asignatura a on a.id=asma.id_asignatura
    JOIN curso_escolar ce on ce.id=asma.id_curso_escolar
    WHERE ce.anyo_inicio BETWEEN 2018 and 2019;
     ```

12. Devuelve un listado con los nombres de **todos** los profesores y los departamentos que tienen vinculados. El listado también debe mostrar aquellos profesores que no tienen ningún departamento asociado. El listado debe devolver cuatro columnas, nombre del departamento, primer apellido, segundo apellido y nombre del profesor. El resultado estará ordenado alfabéticamente de menor a mayor por el nombre del departamento, apellidos y el nombre.

     ```sql
    SELECT DISTINCT d.nombre as 'NOMBRE DEPARTAMENTO',pe.apellido1,pe.apellido2,pe.nombre
    FROM profesor p
    JOIN departamento d on d.id=p.id_departamento
    JOIN persona pe on pe.id=p.id_profesor
    WHERE id_departamento is NULL
    ORDER BY (apellido1) ASC,(apellido2) ASC,(nombre) ASC;
     ```

13. Devuelve un listado con los profesores que no están asociados a un departamento.Devuelve un listado con los departamentos que no tienen profesores asociados.

     ```sql
    SELECT DISTINCT pe.nombre as 'nombre profesor'
    FROM profesor p
    RIGHT JOIN departamento d ON d.id=p.id_departamento
    RIGHT JOIN persona pe on pe.id=p.id_profesor
    WHERE p.id_profesor is NULL;
     ```

14. Devuelve un listado con los profesores que no imparten ninguna asignatura.

     ```sql
    SELECT DISTINCT pe.nombre, a.id as 'asignatura que enseñan'
    FROM profesor p
    LEFT JOIN persona pe on pe.id=p.id_profesor
    left JOIN asignatura a on p.id_profesor=a.id_profesor
    WHERE a.nombre is null;
     ```

15. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

     ```sql
    SELECT DISTINCT a.nombre
    FROM asignatura a
    LEFT JOIN profesor p on p.id_profesor=a.id_profesor
    WHERE p.id_profesor is NULL;
     ```

16. Devuelve un listado con todos los departamentos que tienen alguna asignatura que no se haya impartido en ningún curso escolar. El resultado debe mostrar el nombre del departamento y el nombre de la asignatura que no se haya impartido nunca.

     ```sql
    SELECT DISTINCT *
    FROM asignatura a
    LEFT JOIN profesor p on p.id_profesor=a.id_profesor
    LEFT JOIN departamento d on d.id=p.id_departamento
    LEFT JOIN alumno_se_matricula_asignatura asma on a.id=asma.id_asignatura
    LEFT JOIN curso_escolar ce on ce.id=asma.id_curso_escolar
    WHERE ce.id is null;
     ```

17. Devuelve el número total de **alumnas** que hay.

     ```sql
    SELECT *
    FROM alumno_se_matricula_asignatura;

    SELECT DISTINCT p.nombre,p.apellido1,p.apellido2
    FROM alumno_se_matricula_asignatura asma
    JOIN persona p on p.id=asma.id_alumno
    WHERE p.sexo= 'M'
     ```

18. Calcula cuántos alumnos nacieron en `1999`.

     ```sql
    SELECT *
    FROM persona;

    SELECT nombre,fecha_nacimiento 
    FROM persona
    WHERE fecha_nacimiento LIKE '1999%';
     ```

19. Calcula cuántos profesores hay en cada departamento. El resultado sólo debe mostrar dos columnas, una con el nombre del departamento y otra con el número de profesores que hay en ese departamento. El resultado sólo debe incluir los departamentos que tienen profesores asociados y deberá estar ordenado de mayor a menor por el número de profesores.

     ```sql
    SELECT DISTINCT d.nombre,COUNT(p.id_profesor) as 'profesores asociados'
    FROM profesor p
    JOIN departamento d on d.id=p.id_departamento
    JOIN persona pe on pe.id=p.id_profesor
    GROUP BY d.nombre;
     ```

20. Devuelve un listado con todos los departamentos y el número de profesores que hay en cada uno de ellos. Tenga en cuenta que pueden existir departamentos que no tienen profesores asociados. Estos departamentos también tienen que aparecer en el listado.

     ```sql
    SELECT DISTINCT *
    FROM departamento;

    SELECT DISTINCT d.nombre,COUNT(p.id_profesor) as 'profesores asociados'
    FROM profesor p
    RIGHT JOIN departamento d on d.id=p.id_departamento
    LEFT JOIN persona pe on pe.id=p.id_profesor
    GROUP BY d.nombre
    ORDER BY d.nombre asc;
     ```

21. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno. Tenga en cuenta que pueden existir grados que no tienen asignaturas asociadas. Estos grados también tienen que aparecer en el listado. El resultado deberá estar ordenado de mayor a menor por el número de asignaturas.

     ```sql
    SELECT DISTINCT g.nombre as 'grado',COUNT(a.id)
    FROM grado g
    LEFT JOIN asignatura a on g.id=a.id_grado
    GROUP BY g.nombre
    ORDER BY g.nombre asc; 
     ```

22. Devuelve un listado con el nombre de todos los grados existentes en la base de datos y el número de asignaturas que tiene cada uno, de los grados que tengan más de `40` asignaturas asociadas.

     ```sql
    SELECT DISTINCT g.nombre as 'grado',COUNT(a.id)
    FROM grado g
    LEFT JOIN asignatura a on g.id=a.id_grado
    GROUP BY g.nombre
    HAVING COUNT(a.id)>40
    ORDER BY g.nombre asc;
     ```

23. Devuelve un listado que muestre el nombre de los grados y la suma del número total de créditos que hay para cada tipo de asignatura. El resultado debe tener tres columnas: nombre del grado, tipo de asignatura y la suma de los créditos de todas las asignaturas que hay de ese tipo. Ordene el resultado de mayor a menor por el número total de crédidos.

     ```sql
    SELECT g.nombre AS nombre_grado, a.tipo, SUM(a.creditos) AS total_creditos
    FROM grado g
    INNER JOIN asignatura a ON g.id = a.id_grado
    GROUP BY g.nombre, a.tipo
    ORDER BY total_creditos DESC;
     ```

24. Devuelve un listado que muestre cuántos alumnos se han matriculado de alguna asignatura en cada uno de los cursos escolares. El resultado deberá mostrar dos columnas, una columna con el año de inicio del curso escolar y otra con el número de alumnos matriculados.

     ```sql
    SELECT ce.anyo_inicio, COUNT(DISTINCT am.id_alumno) AS num_alumnos_matriculados
    FROM curso_escolar ce
    LEFT JOIN alumno_se_matricula_asignatura am 
    ON ce.id=am.id_curso_escolar
    GROUP BY ce.anyo_inicio;
     ```

25. Devuelve un listado con el número de asignaturas que imparte cada profesor. El listado debe tener en cuenta aquellos profesores que no imparten ninguna asignatura. El resultado mostrará cinco columnas: id, nombre, primer apellido, segundo apellido y número de asignaturas. El resultado estará ordenado de mayor a menor por el número de asignaturas.

     ```sql
    SELECT p.id AS id_profesor, p.nombre, p.apellido1, p.apellido2, COUNT(a.id) AS num_asignaturas
    FROM persona p
    LEFT JOIN profesor pr ON p.id = pr.id_profesor
    LEFT JOIN asignatura a ON pr.id_profesor = a.id_profesor
    WHERE p.tipo = 'profesor'
    GROUP BY p.id, p.nombre, p.apellido1, p.apellido2
    ORDER BY num_asignaturas DESC;
     ```

26. Devuelve todos los datos del alumno más joven.

     ```sql
    SELECT *
    FROM persona
    WHERE tipo = 'alumno'
    ORDER BY fecha_nacimiento ASC
    LIMIT 1;

    SELECT *
    FROM persona
    WHERE tipo = 'alumno'
    AND fecha_nacimiento = (       
    SELECT MIN(fecha_nacimiento)
    FROM persona
    WHERE tipo = 'alumno'
    );
     ```

27. Devuelve un listado con los profesores que no están asociados a un departamento.

     ```sql
    SELECT *
    FROM persona p
    LEFT JOIN profesor pr ON p.id = pr.id_profesor
    WHERE pr.id_departamento IS NULL AND p.tipo = 'profesor';

    SELECT *
    FROM persona
    WHERE tipo = 'profesor'
    AND id NOT IN (
    SELECT id_profesor
    FROM profesor
    );
     ```

28. Devuelve un listado con los departamentos que no tienen profesores asociados.

     ```sql
    SELECT d.*
    FROM departamento d
    LEFT JOIN profesor pr ON d.id = pr.id_departamento
    WHERE pr.id_departamento IS NULL;

    SELECT *
    FROM departamento
    WHERE id NOT IN (
    SELECT id_departamento
    FROM profesor
    );  
     ```

29. Devuelve un listado con los profesores que tienen un departamento asociado y que no imparten ninguna asignatura.

     ```sql
    SELECT p.id AS id_profesor, p.nombre, p.apellido1, p.apellido2
    FROM persona p
    JOIN profesor pr ON p.id = pr.id_profesor
    LEFT JOIN asignatura a ON pr.id_profesor = a.id_profesor
    WHERE pr.id_departamento IS NOT NULL AND a.id_profesor IS NULL;

    SELECT p.id AS id_profesor, p.nombre, p.apellido1, p.apellido2
    FROM persona p
    JOIN profesor pr ON p.id = pr.id_profesor
    WHERE pr.id_departamento IS NOT NULL
    AND pr.id_profesor NOT IN (
    SELECT id_profesor
    FROM asignatura
    );
     ```

30. Devuelve un listado con las asignaturas que no tienen un profesor asignado.

     ```sql
    SELECT *
    FROM asignatura
    WHERE id_profesor IS NULL;
     ```

31. Devuelve un listado con todos los departamentos que no han impartido asignaturas en ningún curso escolar.

     ```sql
    SELECT d.*
    FROM departamento d
    LEFT JOIN profesor pr 
    ON d.id = pr.id_departamento
    LEFT JOIN asignatura a 
    ON pr.id_profesor = a.id_profesor
    LEFT JOIN alumno_se_matricula_asignatura am 
    ON a.id = am.id_asignatura
    WHERE am.id_curso_escolar IS NULL;

    SELECT *
    FROM departamento
    WHERE id NOT IN (
    SELECT id_departamento
    FROM profesor
    JOIN asignatura 
    ON profesor.id_profesor = asignatura.id_profesor
    JOIN alumno_se_matricula_asignatura 
    ON asignatura.id = alumno_se_matricula_asignatura.id_asignatura
    );
     ```