# DB GESTORE EVENTI

## Selezionare tutti gli eventi gratis, cioè con prezzo nullo (19)

```sql
SELECT * 
FROM `events` 
WHERE `price` IS NULL;
```

## Selezionare tutte le location in ordine al fabetico (82)

```sql
SELECT * 
FROM `locations` 
ORDER BY `name` ASC;
```

## Selezionare tutti gli eventi che costano meno di 20 euro e durano meno di 3 ore (38)

```sql
SELECT * 
FROM `events` 
WHERE `price`< '20.00' 
AND HOUR(`duration`) < 3;
```

<!-- NB: HOUR() funzione: estrapola le ore da un dato di tipo TIME o DATETIME, per esempio se scrivo HOUR('02:30:00') il risultato sarà 2. -->

## Selezionare tutti gli eventi di dicembre 2023 (25)

```sql
SELECT * 
FROM `events` 
WHERE MONTH(`start`) = 12 
AND YEAR(`start`) = 2023;
```

<!-- NB: YEAR() e MONTH() funzioni: estrapolano rispettivamente anno e mese da un dato di tipo DATE o DATETIME, per esempio se scrivo YEAR('2023-06-17') il risultato sarà 2023. Oppure se scrivo MONTH('2023-06-17') il risultato sarà 6. -->

## Selezionare tutti gli eventi con una durata maggiore alle 2 ore (823)

```sql
SELECT * 
FROM `events` 
WHERE HOUR(`duration`) > 2;
```

## Selezionare tutti gli eventi, mostrando nome, data di inizio, ora di inizio, ora di fine e durata totale (1040)

```sql
SELECT `name`, 
DATE(`start`) 
AS `date`, 
TIME(`start`) 
AS `at`, `duration`, 
ADDTIME(HOUR(`start`), `duration`) 
AS `ending time` 
FROM `events`;
```
<!-- NB: con AS creo un alias per la colonna rinominandola come preferisco, ADDTIME mi permette di sommare tempo, con HOUR estrapolo solo l'ora e non visualizzo la data all'interno della colonna start
 -->

## Selezionare tutti gli eventi aggiunti da “Fabiano Lombardo” (id: 1202) (132)

```sql
SELECT * 
FROM `events` 
INNER JOIN `users` 
ON `events`.`user_id` = `users`.`id` 
WHERE `users`.`first_name` = 'Fabiano' 
AND `users`.`last_name` = 'Lombardo';
```

<!-- NB: INNER JOIN unisce due tabelle ON seleziona quali campi della seconda tabella voglio aggiungere alla prima.
 -->

## Selezionare il numero totale di eventi per ogni fascia di prezzo (81)

```sql
SELECT COUNT(`id`) 
AS `number of events`, `price` 
FROM `events` 
GROUP BY `price`;
```

## Selezionare tutti gli utenti admin ed editor (9)

```sql
SELECT * 
FROM `roles` 
INNER JOIN `users` 
ON `roles`.`id` = `users`.`role_id` 
WHERE `roles`.`name` = 'Admin' 
OR `roles`.`name` = 'Editor';
```

## Selezionare tutti i concerti (eventi con il tag “concerti”) (72)

```sql
SELECT * 
FROM `events` 
INNER JOIN `event_tag` 
ON `event_tag`.`event_id` = `events`.`id` 
INNER JOIN `tags` 
ON `tags`.`id` = `event_tag`.`tag_id` 
WHERE `tags`.`name` = 'Concerti';
```
<!-- NB: devo prima collegare la tabella pivot (event_tag) che sta tra la tabella eventi e la tabella tags.
 -->

## Selezionare tutti i tag e il prezzo medio degli eventi a loro collegati (11)

```sql
SELECT `tags`.`name`, AVG(`events`.`price`) 
AS `average_price` 
FROM `tags` 
INNER JOIN `event_tag` 
ON `event_tag`.`tag_id` = `tags`.`id` 
INNER JOIN `events` 
ON `events`.`id` = `event_tag`.`event_id` 
GROUP BY `tags`.`id`;
```

<!-- NB: AVG serve per trovare la media in una colonna di valori
 -->

## Selezionare tutte le location e mostrare quanti eventi si sono tenuti in ognuna di esse (82)

<!-- 
- selezionare tutte le locations (SELECT)
- unire la tabella degli eventi (INNER JOIN)
- contare gli eventi (COUNT())
- suddividere in base alla location (GROUP BY)
-->

```sql
SELECT `locations`.`name`, COUNT(`events`.`id`) 
AS `number_of_events` 
FROM `locations` 
INNER JOIN `events` 
ON `events`.`location_id` = `locations`.`id` 
GROUP BY `locations`.`id`;
```

## Selezionare tutti i partecipanti per l’evento “Concerto Classico Serale” (slug: concerto-classico-serale, id: 34) (30)

<!-- 
- Selezionare tutti i partecipanti
- unire la tabella events 
- unire la tabella bookings
- filtrare solo gli eventi che si chiamano "Concerto Classico Serale" (WHERE)
-->

```sql
SELECT * 
FROM `users` 
INNER JOIN `bookings` 
ON `users`.`id` = `bookings`.`user_id`
INNER JOIN `events` 
ON `bookings`.`event_id` = `events`.`id`
WHERE `events`.`name` = 'Concerto Classico Serale';
```

## Selezionare tutti i partecipanti all’evento “Festival Jazz Estivo” (slug: festival-jazz-estivo, id: 2) specificando nome e cognome (13)

<!-- 
- Selezionare tutti i partecipanti (SELECT)
- unire la tabella bookings (INNER JOIN)
- unire la tabella events 
- filtrare solo gli eventi che si chiamano "Festival Jazz Estivo" (WHERE)
- selezionare solo nomi e cognomi dei partecipanti (SELECT)
-->

```sql
SELECT `first_name`, `last_name` 
FROM `users` 
INNER JOIN `bookings` 
ON `users`.`id` = `bookings`.`user_id` 
INNER JOIN `events` 
ON `bookings`.`event_id` = `events`.`id` 
WHERE `events`.`name` = 'Festival Jazz Estivo';
```

## Selezionare tutti gli eventi sold out (dove il totale delle prenotazioni è uguale ai biglietti totali per l’evento) (18)

- joinare events e bookings
- contare le prenotazioni per ogni evento
- se le prenotazioni sono = al num. massimo di biglietti allora SOLD OUT

```sql
-- SELECT * FROM `events` INNER JOIN `bookings` ON `events`.`id` = `bookings`.`event_id` WHERE `events`.`total_tickets` = 1500;
```

## Selezionare tutte le location in ordine per chi ha ospitato più eventi (82)

```sql

```
