USE sakila;

/* Ejercicios

Base de Datos Sakila:

Para este ejerccio utilizaremos la BBDD Sakila que hemos estado utilizando durante el repaso de SQL. Es
una base de datos de ejemplo que simula una tienda de alquiler de películas. Contiene tablas como film
(películas), actor (actores), customer (clientes), rental (alquileres), category (categorías), entre otras.
Estas tablas contienen información sobre películas, actores, clientes, alquileres y más, y se utilizan para
realizar consultas y análisis de datos en el contexto de una tienda de alquiler de películas. */

-- 1. Selecciona todos los nombres de las películas sin que aparezcan duplicados.

		    SELECT DISTINCT (title)
		    FROM film_text;

  <img width="685" alt="Eje1" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/2e21485e-98f7-4b95-b928-7bde4fc4b266">


-- 2. Muestra los nombres de todas las películas que tengan una clasificación de "PG-13".

		SELECT title,rating
		FROM film
		WHERE rating ='pg-13';

  <img width="503" alt="Eje2" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/68ef98e0-577b-46f4-83cd-f6bde975c35b">


/* 3. Encuentra el título y la descripción de todas las películas que contengan la palabra "amazing" en
su descripción. */

		SELECT title, description
		from film
		WHERE description LIKE '%amazing%';

  <img width="453" alt="Eje3" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/a93ca17f-082c-4784-860c-1a668532ac42">


-- 4. Encuentra el título de todas las películas que tengan una duración mayor a 120 minutos.

		SELECT title, length
		from film
		WHERE length > 120;

  <img width="498" alt="Eje4" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/6d393f91-3b29-42b9-9a07-629d3f810d66">


-- 5. Recupera los nombres de todos los actores.

		SELECT first_name,last_name 
		FROM actor;

  <img width="495" alt="Eje5" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/4c425abe-d1ad-42f1-8335-14ff7354c61d">


-- 6. Encuentra el nombre y apellido de los actores que tengan "Gibson" en su apellido.

		SELECT first_name,last_name 
		FROM actor
		WHERE last_name LIKE 'Gibson';

  
  <img width="455" alt="Eje6" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/d0fb7f6a-41e9-4420-b511-797e82ca80a4">


-- 7. Encuentra los nombres de los actores que tengan un actor_id entre 10 y 20.

		SELECT actor_id,CONCAT(first_name,' ', last_name) AS Nombre_actores
		FROM actor  
		WHERE actor_id BETWEEN 10 AND 20;

  <img width="441" alt="Eje7" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/fca2e01e-911a-4a69-b736-792d8f1b8f1a">


/* 8. Encuentra el título de las películas en la tabla film que no sean ni "R" ni "PG-13" en cuanto a su
clasificación*/

		SELECT title, rating
		FROM film
		WHERE rating NOT IN ('R','PG-13');

  <img width="455" alt="Eje8" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/d8630235-5488-49b4-a265-e770093dd27b">



/* 9. Encuentra la cantidad total de películas en cada clasificación de la tabla film y muestra la
clasificación junto con el recuento.*/

		SELECT COUNT(film_id) AS films_ids,rating
		FROM film
		GROUP BY rating;

  <img width="435" alt="Eje9" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/ac8aa897-2186-470e-b755-f563a1a0b277">



/* 10. Encuentra la cantidad total de películas alquiladas por cada cliente y muestra el ID del cliente, su
nombre y apellido junto con la cantidad de películas alquiladas.*/

-- En esta selección no se tiene en cuenta si el cliente ha cogido varias veces la misma pelicula.

		SELECT COUNT(r.customer_id) AS Number_of_rentals, CONCAT(first_name,' ',last_name) AS Customer_Name
		FROM customer AS c
		INNER JOIN rental AS r ON c.customer_id = r.customer_id
		GROUP BY r.customer_id;

-- Lo hacemos para ver el total de alquileres con peliculas distintas.

		SELECT COUNT(distinct(i.film_id)) as film_identity, CONCAT(first_name,' ',last_name) AS Customer_Name
		FROM rental as r
		INNER JOIN inventory as i ON r.inventory_id =i.inventory_id
		INNER JOIN customer as c ON c.customer_id =r.customer_id
		GROUP BY c.customer_id;

  <img width="468" alt="Eje10" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/13b3046f-b1a7-4107-a0a8-f02469dc16b0">


/* 11. Encuentra la cantidad total de películas alquiladas por categoría y muestra el nombre de la
categoría junto con el recuento de alquileres.*/

		SELECT cat.name, COUNT(i.film_id) AS Total_alquilado
		FROM rental AS r
		INNER JOIN inventory AS i ON r.inventory_id = i.inventory_id
		INNER JOIN film_category AS fcat ON fcat.film_id= i.film_id
		INNER JOIN category AS cat ON cat.category_id = fcat.category_id
		GROUP BY cat.name;

  <img width="440" alt="Eje11" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/11701453-0857-4268-979a-6353ada8fdad">


    /* Razonamiento
			rental -- rental id. -- inventory_id
            inventario --  film_id,  -- inventario_id
			category -- cat.name -- category id
			film_category -- category_id	

    */


/* 12. Encuentra el promedio de duración de las películas para cada clasificación de la tabla film y
muestra la clasificación junto con el promedio de duración.*/


		SELECT ROUND(AVG(length)) AS Average_Length, rating
		FROM film
		GROUP BY rating;

  <img width="451" alt="Eje12" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/bedf7e28-5e27-4462-a95b-815f3d1b2a60">


-- 13. Encuentra el nombre y apellido de los actores que aparecen en la película con title "Indian Love".

		SELECT CONCAT (act.first_name,' ',act.last_name) AS Actors_in_movie, act.actor_id
		FROM actor AS act
		INNER JOIN film_actor AS factor ON act.actor_id =factor.actor_id
		INNER JOIN film AS f ON f.film_id = factor.film_id
		WHERE f.title = 'Indian Love'
		ORDER BY act.first_name ASC;


  <img width="446" alt="Eje13" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/53ee702c-e4dc-47e3-bf47-8e1db78adb92">


-- 14. Muestra el título de todas las películas que contengan la palabra "dog" o "cat" en su descripción.

		SELECT film_id, title, description
        FROM film
        WHERE description LIKE '%dog%' OR description LIKE '%cat%';

  <img width="460" alt="Eje14" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/af183d8b-2df2-48cb-bfe8-4bf7186658d9">

        
-- 15. Hay algún actor o actriz que no apareca en ninguna película en la tabla film_actor.

		SELECT *
		FROM actor
		WHERE actor_id NOT IN (SELECT actor_id FROM film_actor);

  <img width="445" alt="Eje15" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/9808ebb6-a607-4532-965f-b6f98a64ebaa">

    
		-- Comprobacion:
		
			SELECT COUNT(actor_id) FROM actor;
			SELECT COUNT(DISTINCT(actor_id)) FROM film_actor;

-- 16. Encuentra el título de todas las películas que fueron lanzadas entre el año 2005 y 2010.

		SELECT film_id,title,release_year
        FROM film
        WHERE release_year BETWEEN 2005 AND 2010;

  <img width="457" alt="Eje16" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/a0ff3fdd-51b4-4026-854f-31561f2b8c7e">


-- 17. Encuentra el título de todas las películas que son de la misma categoría que "Family".

		SELECT f.film_id, f.title, cat.name
        FROM film AS f
        INNER JOIN film_category AS fcat ON f.film_id = fcat.film_id
        INNER JOIN category AS cat ON fcat.category_id = cat.category_id
        WHERE cat.name ='Family';

  <img width="438" alt="Eje17" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/e3816261-e108-4eb9-9272-5716e64d606c">

        
       /* Razonamiento  
        film -- film_id -- title
        film_category -- film_id-- category id
        category -- category_id -- name */
        
-- 18. Muestra el nombre y apellido de los actores que aparecen en más de 10 películas.

		SELECT COUNT(film_id) AS film_sum, CONCAT(first_name,' ',last_name) AS Actors_Names,factor.actor_id
		FROM film_actor AS factor
		INNER JOIN actor AS act ON factor.actor_id = act.actor_id
		GROUP BY factor.actor_id
        HAVING film_sum >10;

  <img width="455" alt="Eje18" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/f5084a5e-961f-4582-96db-a96b94e887cc">


/* 19. Encuentra el título de todas las películas que son "R" y tienen una duración mayor a 2 horas en la
	tabla film.*/
    
		SELECT film_id, title, rating,length
        FROM film
        WHERE rating ='R' AND length >120;

  <img width="437" alt="Eje19" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/04a4cf22-974e-43e1-a724-0e27c622917e">

          
    
/* 20. Encuentra las categorías de películas que tienen un promedio de duración superior a 120
 minutos y muestra el nombre de la categoría junto con el promedio de duración. */
 
        SELECT fcat.category_id, name,ROUND(AVG(length)) AS length_average
        FROM film AS f
        LEFT JOIN film_category AS fcat ON fcat.film_id = f.film_id
        LEFT JOIN category ON category.category_id = fcat.category_id
        GROUP BY category_id
        HAVING length_average >120;
 
		    -- film -- length -- film_id
        -- film_category -- film_id -- category_id
        -- category -- name -- category_id
	
		SELECT fcat.category_id, name,ROUND(AVG(length)) AS length_average
        FROM film AS f
        INNER JOIN film_category AS fcat ON fcat.film_id = f.film_id
        INNER JOIN category ON category.category_id = fcat.category_id
        GROUP BY category_id
        HAVING length_average >120;

    
  <img width="441" alt="Eje20" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/ae9d044d-a427-417e-a2e6-c67dc5871135">


/* 21. Encuentra los actores que han actuado en al menos 5 películas y muestra el nombre del actor
junto con la cantidad de películas en las que han actuado.*/

        SELECT factor.actor_id, CONCAT (first_name,' ' ,last_name) AS Actors_names , COUNT(factor.film_id) AS film_add
        FROM actor AS act
        INNER JOIN film_actor AS factor ON act.actor_id = factor.actor_id
        GROUP BY factor.actor_id
        HAVING film_add >5;

  <img width="447" alt="Eje21" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/2a619d61-0e65-4c85-96fc-4abedc295822">


		-- Razonamiento--
		-- actor -- actor_id -- first_name -- last_name
        -- film_actor -- actor_id -- film_id




/* 22. Encuentra el título de todas las películas que fueron alquiladas por más de 5 días. Utiliza una
subconsulta para encontrar los rental_ids con una duración superior a 5 días y luego selecciona
las películas correspondientes.*/
        
		SELECT film_id, title
		FROM film_text
		WHERE film_id IN (SELECT film_id 
						  FROM inventory WHERE inventory_id IN (SELECT inventory_id 
																FROM rental 
																WHERE DATEDIFF (return_date,rental_date) >5));
  
  <img width="449" alt="Eje22" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/3109a355-698e-4880-94a1-74f61e679dd0">

        

		-- Razonamiento -- 
        -- rental -- rental_id -- rental_date -- return_date -- inventory_id
        -- film_text -- film_id -- title
        -- inventory -- inventory_id -- film_id


/* 23. Encuentra el nombre y apellido de los actores que no han actuado en ninguna película de la
categoría "Horror". Utiliza una subconsulta para encontrar los actores que han actuado en
películas de la categoría "Horror" y luego exclúyelos de la lista de actores.
*/
             
		SELECT CONCAT (first_name,' ' ,last_name) AS Actors_names
		FROM actor
		WHERE actor_id NOT IN (SELECT actor_id
							   FROM film_actor   
							   WHERE film_id IN (SELECT film_id
												FROM film_category
												WHERE category_id =  (SELECT category_id
																	  FROM category
																	  WHERE name = 'Horror')));



    
		-- Razonamiento
		-- actor -- firt_name -- last_name -- actor_id
        -- film_actor -- actor_id -- film_id
        -- film_category -- film_id -- category_id
		-- category -- category_id -- name

-- BONUS:

/* 24. Encuentra el título de las películas que son comedias y tienen una duración mayor a 180
minutos en la tabla film.*/

	SELECT f.film_id, f.title, f.length, cat.name
    FROM film AS f
    INNER JOIN film_category AS fcat ON f.film_id = fcat.film_id
    INNER JOIN category AS cat ON fcat.category_id = cat.category_id
    WHERE f.length >180 AND cat.name = 'Comedy';

    
  <img width="447" alt="Eje24" src="https://github.com/spinelf/promo-H-DA-modulo2-evaluacion-final-SilviaPinel/assets/99440874/344bc95e-a1bc-4bf3-8983-b2c0a36d7814">


/* 25. BONUS: Encuentra todos los actores que han actuado juntos en al menos una película. La
consulta debe mostrar el nombre y apellido de los actores y el número de películas en las que
han actuado juntos.*/


	




