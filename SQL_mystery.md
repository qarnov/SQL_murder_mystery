url - https://mystery.knightlab.com/

Given hint:You vaguely remember that the crime was a ​murder​ that occurred sometime on ​Jan.15, 2018​ and that it took place in ​SQL City​. 

#Finding the table:

SELECT name 
FROM sqlite_master
where type = 'table'

#The table is  crime_scene_report

#We know that the crime occured on Jan.15, 2018, and city is SQL city and the type is murder

SELECT *
FROM crime_scene_report
WHERE date =20180115
AND city = 'SQL City'
AND type = 'murder'

           
"Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave"."


#The 1st guy's info:
SELECT *
 FROM person 
 WHERE address_street_name = 'Northwestern Dr' ORDER BY address_number DSC
id	name	license_id	address_number	address_street_name	ssn
14887	Morty Schapiro	118009	4919	Northwestern Dr	111564949

#and for the 2nd guy in the description
SELECT *
FROM person              
WHERE address_street_name = 'Franklin Ave' AND name ='Annabel Miller'
id	name	license_id	address_number	address_street_name	ssn
16371	Annabel Miller	490173	103	Franklin Ave	318771143


#from the interview table we can see people's interview to the police so we check these two person's ID in the table 


SELECT *
FROM interview
WHERE person_id = 14887

person_id	transcript
14887	I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

SELECT *
FROM interview
WHERE person_id = 16371

person_id	transcript
16371	I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

#checking Get Fit Now Gym 
SELECT *
FROM get_fit_now_member
WHERE id like '48Z%'
id	person_id	name	membership_start_date	membership_status
48Z38	49550	Tomas Baisley	20170203	silver
48Z7A	28819	Joe Germuska	20160305	gold
48Z55	67318	Jeremy Bowers	20160101	gold

#checking car plate number 
SELECT *
FROM drivers_license
WHERE plate_number like '%H42W%'
id	age	height	eye_color	hair_color	gender	plate_number	car_make	car_model
183779	21	65	blue	blonde	female	H42W0X	Toyota	Prius
423327	30	70	brown	brown	male	0H42W2	Chevrolet	Spark LS
664760	21	71	black	black	male	4H42WR	Nissan	Altima

#Checking the people who worked out at the Gym on Januray 9th with the ID similar to 48Z 
SELECT *
FROM get_fit_now_check_in
WHERE membership_id like '%48Z%' AND check_in_date = 20180109

membership_id	check_in_date	check_in_time	check_out_time
48Z7A	20180109	1600	1730
48Z55	20180109	1530	1700

#Now it is safe to deduce that Either it is Joe Germuska or Jeremy Bowers since the membership ID belongs to them 





