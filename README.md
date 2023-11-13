# Foreigns-visitors-in-Japan
CTAS, Windows Function, Case when
/*case when, CTAS
Calculating the total number of visitors for a given country 
and creating a new column with 4 groups
 */

create table table_ranking as select 
	v.* 
	,case 
		when total_number_of_visitors <= 10000 then 'group 4'
		when total_number_of_visitors > 10000 
		and total_number_of_visitors < 100000 then 'group 3'
		when total_number_of_visitors >= 100000 
		and total_number_of_visitors < 1000000 then 'group 2'
		else 'group 1'
	end as visitors_type_group_by_country
from 
(select
	`Country/Area`
	,sum(visitor)          as total_number_of_visitors
from visitors_by_moth vbm 
group by 1) v 
order by total_number_of_visitors  desc

/*Calculations based on the new table and creation of a ranking */
select 
	visitors_type_group_by_country
	,count(`Country/Area`) 										  as number_of_country_by_groups
	,sum(total_number_of_visitors)                                as number_of_visitors_by_groups
	,row_number () over (order by sum(total_number_of_visitors))  as ranking
from table_ranking
group by 1
