# Assessment-Answers
Below are queries to answer number 1.

CREATE TABLE public."CRMcclog"(datereceived VARCHAR(100),complaind_id VARCHAR(100),rand_client VARCHAR(100),phonefinal VARCHAR(100),vru_line VARCHAR(100),call_id VARCHAR(100),priority VARCHAR(100),type VARCHAR(100),outcome VARCHAR(100),server VARCHAR(100),ser_start TIME,ser_exit TIME,ser_time TIME)
COPY public."CRMcclog" FROM 'C:\Users\User\Desktop\CRMCClogs.csv' DELIMITER ',' CSV HEADER;

CREATE TABLE public."CRMevent"(Date_received VARCHAR(100),Product VARCHAR(100),Subproduct VARCHAR(100),Issue VARCHAR(100),Subissue VARCHAR(100),Consumer_complaint_narrative VARCHAR(100),Tags VARCHAR(100),Consumer_consent_provided VARCHAR(100),Submitted_via VARCHAR(100),Date_sent_to_company VARCHAR(100),Company_response_to_ consumer VARCHAR(100),Timely_response VARCHAR(100),Consumer_disputed VARCHAR(100),Complaint_ID VARCHAR(100),Client_ID VARCHAR(100))
COPY public."CRMevent" FROM 'C:\Users\User\Desktop\CRMevents.csv' DELIMITER ',' CSV HEADER;

# /*avg service time for each product*/
select e.product,
AVG(l.ser_time) AS avg_resolve_time
from public."CRMevent" as e
join
public."CRMcclog" as l
on e.complaint_id = l.complaind_id
GROUP BY e.product

# /*avg service time for each subproduct*/
select e.subproduct,
AVG(l.ser_time) AS avg_resolve_time
from public."CRMevent" as e
join
public."CRMcclog" as l
on e.complaint_id = l.complaind_id
Where subproduct is not null /*based on observation, null means credit card*/
GROUP BY  e.subproduct

# /*avg service time for each type of issue*/
select e.issue,
AVG(l.ser_time) AS avg_resolve_time
from public."CRMevent" as e
join
public."CRMcclog" as l
on e.complaint_id = l.complaind_id
GROUP BY  e.issue
