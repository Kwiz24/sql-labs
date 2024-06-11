# sql-labs
karlonheileman=# CREATE DATABASE nfl; -- create the nfl database
CREATE DATABASE
karlonheileman=# \connect nfl;                               
You are now connected to database "nfl" as user "karlonheileman".
nfl=# 
nfl=# DROP TABLE IF EXISTS players;
DROP TABLE IF EXISTS teams;

CREATE TABLE teams (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    stadium VARCHAR(255),
    division VARCHAR(255),
    conference VARCHAR(255),
    head_coach VARCHAR(255),
    active BOOLEAN
);

CREATE TABLE players (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    position VARCHAR(255),
    salary INTEGER,
    team_id INTEGER REFERENCES teams
);
NOTICE:  table "players" does not exist, skipping
DROP TABLE
NOTICE:  table "teams" does not exist, skipping
DROP TABLE
CREATE TABLE
CREATE TABLE
nfl=# INSERT INTO teams (name, stadium, head_coach, conference, division, active) VALUES
('Buffalo Bills', 'Ralph Wilson Stadium', 'Doug Marrone', 'AFC', 'East', true),
('Miami Dolphins', 'Sun Life Stadium', 'Joe Philbin', 'AFC', 'East', true),
('New England Patriots', 'Gillette Stadium', 'Bill Belichick', 'AFC', 'East', true),
('New York Jets', 'MetLife Stadium', 'Rex Ryan', 'AFC', 'East', true),
('Baltimore Ravens', 'M&T Bank Stadium', 'John Harbaugh', 'AFC', 'North', true),
('Cincinnati Bengals', 'Paul Brown Stadium', 'Marvin Lewis', 'AFC', 'North', true),
('Cleveland Browns', 'FirstEnergy Stadium', 'Mike Pettine', 'AFC', 'North', true),
('Pittsburgh Steelers', 'Heinz Field', 'Mike Tomlin', 'AFC', 'North', true),
('Houston Texans', 'NRG Stadium', 'Bill OBrien', 'AFC', 'South', true),
('Indianapolis Colts', 'Lucas Oil Stadium', 'Chuck Pagano', 'AFC', 'South', true),
('Jacksonville Jaguars', 'EverBank Field', 'Gus Bradley', 'AFC', 'South', true),
('Tennessee Titans', 'LP Field', 'Ken Whisenhunt', 'AFC', 'South', true),
('Denver Broncos', 'Sports Authority Field', 'John Fox', 'AFC', 'West', true),
('Kansas City Chiefs', 'Arrowhead Stadium', 'Andy Reid', 'AFC', 'West', true),
('Oakland Raiders', 'O.co Coliseum', 'Tony Sparano', 'AFC', 'West', true),
('San Diego Chargers', 'Qualcomm Stadium', 'Mike McCoy', 'AFC', 'West', true),
('Dallas Cowboys', 'AT&T Stadium', 'Jason Garrett', 'NFC', 'East', true),
('New York Giants', 'MetLife Stadium', 'Tom Coughlin', 'NFC', 'East', true),
('Philadelphia Eagles', 'Lincoln Financial Field', 'Chip Kelly', 'NFC', 'East', true),
('Washington Redskins', 'FedExField', 'Jay Gruden', 'NFC', 'East', true),
('Chicago Bears', 'Soldier Field', 'Marc Trestman', 'NFC', 'North', true),
('Detroit Lions', 'Ford Field', 'Jim Caldwell', 'NFC', 'North', true),
('Green Bay Packers', 'Lambeau Field', 'Mike McCarthy', 'NFC', 'North', true),
('Minnesota Vikings', 'TCF Bank Stadium', 'Mike Zimmer', 'NFC', 'North', true),
('Atlanta Falcons', 'Georgia Dome', 'Mike Smith', 'NFC', 'South', true),
('Carolina Panthers', 'Bank of America Stadium', 'Ron Rivera', 'NFC', 'South', true),
('New Orleans Saints', 'Mercedes-Benz Superdome', 'Sean Payton', 'NFC', 'South', true),
('Tampa Bay Buccaneers', 'Raymond James Stadium', 'Lovie Smith', 'NFC', 'South', true),
('Arizona Cardinals', 'University of Phoenix Stadium', 'Bruce Arians', 'NFC', 'West', true),
('St. Louis Rams', 'Edward Jones Dome', 'Jeff Fisher', 'NFC', 'West', true),
('San Francisco 49ers', 'Levis Stadium', 'Jim Harbaugh', 'NFC', 'West', true),
('Seattle Seahawks', 'CenturyLink Field', 'Pete Carroll', 'NFC', 'West', true);
INSERT 0 32
nfl=# INSERT INTO players (name, position, salary, team_id) VALUES
('Mario Williams', 'DE', 5900000, 1),
('Drayton Florence', 'CB', 4000000, 1),
('Shawne Merriman', 'LB', 4000000, 1),
('Dwan Edwards', 'DT', 3800000, 1),
('Chris Kelsay', 'DE', 3500000, 1),
('Kyle Williams', 'DT', 3000000, 1),
('Nick Barnett', 'LB', 3000000, 1),
('Spencer Johnson', 'DT', 3000000, 1),
('Ryan Fitzpatrick', 'QB', 2800000, 1),
('Steve Johnson', 'WR', 2500000, 1),
('Tyler Thigpen', 'QB', 2500000, 1),
('Lee Evans (Buyout)', 'WR', 2383334, 1),
('Brad Smith', 'WR', 2250000, 1),
('Fred Jackson', 'RB', 1955000, 1),
('Scott Chandler', 'TE', 1800000, 1),
('George Wilson', 'S', 1725000, 1);
INSERT 0 16
nfl=# psql -d nfl -f schema.sql
psql -d nfl -f teams.sql
psql -d nfl -f players.sql
nfl-# git add .
git commit -m "Commit: NFL db seeded"
nfl-# -- 1. List the names of all NFL teams
SELECT name FROM teams;

-- 2. List the stadium name and head coach of all NFC teams
SELECT stadium, head_coach FROM teams WHERE conference = 'NFC';

-- 3. List the head coaches of the AFC South
SELECT head_coach FROM teams WHERE conference = 'AFC' AND division = 'South';

-- 4. The total number of players in the NFL
SELECT COUNT(*) FROM players;

-- 5. The team names and head coaches of the NFC North and AFC East
SELECT name, head_coach FROM teams WHERE (conference = 'NFC' AND division = 'North') OR (conference = 'AFC' AND division = 'East');

-- 6. The 50 players with the highest salaries
SELECT name, salary FROM players ORDER BY salary DESC LIMIT 50;

-- 7. The average salary of all NFL players
SELECT AVG(salary) FROM players;

-- 8. The names and positions of players with a salary above 10_000_000
SELECT name, position FROM players WHERE salary > 10000000;

-- 9. The player with the highest salary in the NFL
SELECT name, salary FROM players ORDER BY salary DESC LIMIT 1;

-- 10. The name and position of the first 100 players with the lowest salaries
SELECT name, position FROM players ORDER BY salary ASC LIMIT 100;

-- 11. The average salary for a DE in the NFL
SELECT AVG(salary) FROM players WHERE position = 'DE';

-- 12. The names of all the players on the Buffalo Bills
SELECT players.name FROM players INNER JOIN teams ON players.team_id = teams.id WHERE teams.name = 'Buffalo Bills';

-- 13. The total salary of all players on the New York Giants
SELECT SUM(salary) FROM players WHERE team_id = (SELECT id FROM teams WHERE name = 'New York Giants');

-- 14. The player with the lowest salary on the Green Bay Packers
SELECT name, salary FROM players WHERE team_id = (SELECT id FROM teams WHERE name = 'Green Bay Packers') ORDER BY salary ASC LIMIT 1;
ERROR:  syntax error at or near "psql"
LINE 1: psql -d nfl -f schema.sql
        ^
            stadium            |  head_coach   
-------------------------------+---------------
 AT&T Stadium                  | Jason Garrett
 MetLife Stadium               | Tom Coughlin
 Lincoln Financial Field       | Chip Kelly
 FedExField                    | Jay Gruden
 Soldier Field                 | Marc Trestman
 Ford Field                    | Jim Caldwell
 Lambeau Field                 | Mike McCarthy
 TCF Bank Stadium              | Mike Zimmer
 Georgia Dome                  | Mike Smith
 Bank of America Stadium       | Ron Rivera
 Mercedes-Benz Superdome       | Sean Payton
 Raymond James Stadium         | Lovie Smith
 University of Phoenix Stadium | Bruce Arians
 Edward Jones Dome             | Jeff Fisher
 Levis Stadium                 | Jim Harbaugh
 CenturyLink Field             | Pete Carroll
(16 rows)

   head_coach   
----------------
 Bill OBrien
 Chuck Pagano
 Gus Bradley
 Ken Whisenhunt
(4 rows)

 count 
-------
    16
(1 row)

         name         |   head_coach   
----------------------+----------------
 Buffalo Bills        | Doug Marrone
 Miami Dolphins       | Joe Philbin
 New England Patriots | Bill Belichick
 New York Jets        | Rex Ryan
 Chicago Bears        | Marc Trestman
 Detroit Lions        | Jim Caldwell
 Green Bay Packers    | Mike McCarthy
 Minnesota Vikings    | Mike Zimmer
(8 rows)

        name        | salary  
--------------------+---------
 Mario Williams     | 5900000
 Drayton Florence   | 4000000
 Shawne Merriman    | 4000000
 Dwan Edwards       | 3800000
 Chris Kelsay       | 3500000
 Kyle Williams      | 3000000
 Nick Barnett       | 3000000
 Spencer Johnson    | 3000000
 Ryan Fitzpatrick   | 2800000
 Steve Johnson      | 2500000
 Tyler Thigpen      | 2500000
 Lee Evans (Buyout) | 2383334
 Brad Smith         | 2250000
 Fred Jackson       | 1955000
 Scott Chandler     | 1800000
 George Wilson      | 1725000
(16 rows)

         avg          
----------------------
 3007083.375000000000
(1 row)

 name | position 
------+----------
(0 rows)

      name      | salary  
----------------+---------
 Mario Williams | 5900000
(1 row)

        name        | position 
--------------------+----------
 George Wilson      | S
 Scott Chandler     | TE
 Fred Jackson       | RB
 Brad Smith         | WR
 Lee Evans (Buyout) | WR
 Steve Johnson      | WR
 Tyler Thigpen      | QB
 Ryan Fitzpatrick   | QB
 Spencer Johnson    | DT
 Nick Barnett       | LB
 Kyle Williams      | DT
 Chris Kelsay       | DE
 Dwan Edwards       | DT
 Drayton Florence   | CB
 Shawne Merriman    | LB
 Mario Williams     | DE
(16 rows)

         avg          
----------------------
 4700000.000000000000
(1 row)

        name        
--------------------
 Mario Williams
 Drayton Florence
 Shawne Merriman
 Dwan Edwards
 Chris Kelsay
 Kyle Williams
 Nick Barnett
 Spencer Johnson
 Ryan Fitzpatrick
 Steve Johnson
 Tyler Thigpen
 Lee Evans (Buyout)
 Brad Smith
 Fred Jackson
 Scott Chandler
 George Wilson
(16 rows)

 sum 
-----
    
(1 row)

 name | salary 
------+--------
(0 rows)

nfl=# git add nfl.sql
git commit -m "Commit: NFL queries"
nfl-# \d
