* **Olivnder’s inventory prob :**  
  with ct as (  
  select  
  w.id, wp.age,w.power,w.coins\_needed ,  
  ROW\_NUMBER() OVER(PARTITION BY wp.age,w.power ORDER BY w.coins\_needed ASC ) AS R  
  from wands w  
  join wands\_property wp on w.code\=wp.code  
  where wp.is\_evil\=0  
    
  )  
  select id,age,coins\_needed,power  
  from ct  
  WHERE R\=1  
  order by power desc,age desc

  ; 

* Julia asked her students to create some coding challenges. Write a query to print the hacker\_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker\_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.  

   with ct as(  
  SELECT  
  h.hacker\_id, h.name, count(c.challenge\_id) as cnt  
  FROM Hackers h  
  join Challenges c on c.hacker\_id\=h.hacker\_id  
  group by h.hacker\_id, h.name),  
    
  ct1 as (  
  select hacker\_id, name, cnt,count(cnt) over(partition by cnt order by cnt desc ) as cnt1, max(cnt) over() as m  
  from ct),  
  ct2 as(  
  select hacker\_id, name , cnt,cnt1,  
  case  
      when cnt1\>1 and cnt\<\>m then 1  
      else 0  
      end as flag  
  from ct1)  
  select hacker\_id, name, cnt  
  from ct2  
  where flag\=0  
  order by cnt desc, hacker\_id asc  
    
  ;  
    
* You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too\! The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker\_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker\_id. Exclude all hackers with a total score of  from your result.  

   with ct as (  
  select  
      h.hacker\_id, h.name ,s.challenge\_id, max(s.score) as m  
  from hackers h  
  join Submissions s on s.hacker\_id\=h.hacker\_id  
  group by h.hacker\_id, h.name , s.challenge\_id  
  ),  
  ct1 as (  
  select hacker\_id,name,sum(m) as s  
  from ct  
  group by hacker\_id,name)  
    
  select hacker\_id,name, s  
  from ct1  
  where s\<\>0  
  order by s desc, hacker\_id asc  
    
  ;


  

