 ```with mt as (select  
   CAST(created_at as year)   as ano,
            count ( match_id) as matches
           from matches
           group by 1
           order by 1 desc
             ),
ordem as ( select ano,
 matches,
  LAG(matches) OVER (ORDER BY ano ) AS matches_ano_anterior
 from mt
          )
 select ano,
 matches,
 ROUND(
        (matches - matches_ano_anterior) * 100.0 / matches_ano_anterior,
        2
    ) AS crescimento_percentual
 from ordem 
 order by 1 desc;
