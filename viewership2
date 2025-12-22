select* from snowflake_learning_db.public.profile;
select* from snowflake_learning_db.public.viewers;

__#USER profile table#
...........................

CREATE TEMPORARY TABLE users as
    (SELECT
        USERID,
        age,
        
        CASE
            WHEN gender = 'None' THEN 'unspecified'
            WHEN gender IS NULL THEN 'unspecified'
            ELSE gender
        END AS gender,

        CASE
      WHEN Age BETWEEN 0 AND 12 THEN 'kid'
      WHEN Age BETWEEN 13 AND 19 THEN 'teenager'
      WHEN Age BETWEEN 20 AND 35 THEN 'youth'
      WHEN Age BETWEEN 36 AND 55 THEN 'adult'
      WHEN Age BETWEEN 56 AND 70 THEN 'matured adult'
      ELSE 'elderly'
    END AS age_group,
    
        CASE
            WHEN race = 'None' THEN 'unspecified'
            WHEN race = 'other' THEN 'unspecified'
            WHEN race IS NULL THEN 'unspecified'
            ELSE race
        END AS race,
       
       CASE
            WHEN province = 'None' THEN 'unspecified'
            WHEN province IS NULL THEN 'unspecified'
            ELSE province
        END AS province
    
        from snowflake_learning_db.public.profile);

        select* from users;


        ___viewers table___##
   ..............................

        create or replace temp table views as
(SELECT 
coalesce("userid","UserID") as "USERID",
CHANNEL2,

    TO_TIMESTAMP("RECORDDATE2", 'DD/MM/YYYY HH24:MI') AS RECORDDATE2_TS,
    TO_CHAR(TO_TIMESTAMP("RECORDDATE2", 'DD/MM/YYYY HH24:MI'), 'HH24:MI:SS') AS WATCHTIME,
    DAYNAME(RECORDDATE2_TS) as day_name,
MONTHNAME(RECORDDATE2_TS) as month_name,
TO_DATE(RECORDDATE2_TS) as watch_date,

DURATION_2,

CASE
    WHEN WATCHTIME BETWEEN '05:00' AND '11:59' THEN 'morning'
    WHEN WATCHTIME BETWEEN '12:00' AND '17:59' THEN 'afternoon'
    WHEN WATCHTIME BETWEEN '18:00' AND '23:59' THEN 'evening'
    WHEN WATCHTIME BETWEEN '00:00' AND '04:59' THEN 'midnight'
    ELSE 'not specified'
  END AS time_bucket,

  CASE

  WHEN DAY_NAME in ('Mon','Tue','Wed','Thu','Fri') THEN 'Weekday'
  ELSE 'weekend'
  END AS weekly_classification,
 from snowflake_learning_db.public.viewers);
    
select *from views;


------Joining the Tables----
select distinct users.USERID as viewership,
CHANNEL2,
WATCHTIME,
DAY_NAME,
MONTH_NAME,
WATCH_DATE,
time_bucket,
weekly_classification,
users.AGE,
users.GENDER,
users.AGE_GROUP,
users.RACE,
users.PROVINCE,
from views
left join  users on views.USERID =users.USERID;
