# database-checkpoint
```sql
-- Create a table called destinations and populate it with each location's id,
-- name, average_temp, has_beaches, has_mountains, and cost_of_flight.
create table if not exists destinations (
    id serial primary key,
    name varchar(50) not null,
    average_temp integer,
    has_beaches boolean,
    has_mountains boolean,
    cost_of_flight integer
);
--Create a table called airlines and populate it with each airline's name and id.
create table if not exists airlines (
    id serial primary key,
    name varchar(50) not null
);
-- Insert the above items into your list table.
insert into destinations (
        name,
        average_temp,
        cost_of_flight,
        has_beaches,
        has_mountains
    )
values ('Thailand', 82, 765, true, true),
    ('Minnesota', 41, 235, false, false),
    ('New Zealand', 66, 433, true, true),
    ('England', 45, 290, false, false),
    ('Tristan da Cunha', 59, 1304, true, true);
-- Insert airline data
insert into airlines (name)
values ('Spirit'),
    ('Lufthansa'),
    ('Delta'),
    ('Southwest');
-- All of the vacation destinations.
select name
from destinations;
-- All destinations where you can swim at the beach.
select *
from destinations
where has_beaches = true;
-- All destinations where the average temperature is over 60 degrees.
select *
from destinations
where average_temp > 60;
-- All destinations where you can swim at the beach AND go to the mountains.
select *
from destinations
where has_beaches = true
    and has_mountains = true;
-- All destinations where flights cost less than $500 and you can hike in the mountains.
select *
from destinations
where cost_of_flight < 500
    and has_mountains = true;
-- Add an entry for The Bahamas, where the average temperature is 78, it has beaches but no mountains, and the flights cost $345.
insert into destinations (
        name,
        average_temp,
        cost_of_flight,
        has_beaches,
        has_mountains
    )
values ('The Bahamas', 78, 345, true, false);
-- Turns out, the cost of flights to New Zealand has increased!
-- Update New Zealand's entry for flight cost to $1000.
update destinations
set cost_of_flight = 1000
where name = 'New Zealand';
-- Turns out, Minnesota isn't a vacation destination. Please delete it from the database.
delete from destinations
where id = 2;
-- When the data set was written, the author mistakently wrote "England" when they
-- actually meant "Scotland". Please update that entry in the database.
update destinations
set name = 'Scotland'
where id = 4;
-- Create a join table that joins the airlines and the destinations tables by
-- correlating which airlines fly to which destinations.
create table if not exists destinations_airlines (
    destination_id integer references destinations(id),
    airline_id integer references airlines(id)
);
-- [
--   {
--     name: "Spirit",
--     destinations_flown_to: ["New Zealand", "Scotland"]
--   },
--   {
--     name: "Lufthansa",
--     destinations_flown_to: ["Tristan da Cunha", "Scotland", "Thailand"]
--   },
--   {
--     name: "Delta",
--     destinations_flown_to: ["Thailand", "Minnesota", "England", "Scotland"]
--   },
--   {
--     name: "Southwest"
--     destinations_flown_to: ["New Zealeand", "Tristan de Cunha", "Minnesota"]
--   }
-- ]
select id,
    name
from destinations;
select id,
    name
from airlines;
insert into destinations_airlines (airline_id, destination_id)
values (1, 3),
    (1, 4),
    (2, 5),
    (2, 4),
    (2, 1),
    (3, 1),
    (3, 4),
    (4, 3),
    (4, 5);
-- All airlines that fly to New Zealand.
select airlines.id,
    airlines.name,
    destinations.name
from airlines
    inner join destinations_airlines on airlines.id = destinations_airlines.airline_id
    inner join destinations on destinations_airlines.destination_id = destinations.id
where destinations.name = 'New Zealand';
-- All airlines that do NOT fly to Scotland.
select airlines.id,
    airlines.name,
    destinations.name
from airlines
    inner join destinations_airlines on airlines.id = destinations_airlines.airline_id
    inner join destinations on destinations_airlines.destination_id = destinations.id
where destinations.name != 'Scotland';
-- All of the data for all vacation destinations.
select *
from destinations;
```