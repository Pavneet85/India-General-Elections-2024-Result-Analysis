# India General Election 2024 Result Analysis

## About India General Elections: 

India is the largest democracy in the world with a population count of 1.4 billion. General elections were held in India from 19 April to 1 June 2024 in seven phases, to elect all 543 members of the Lok Sabha. More than 968 million people out of a population of 1.4 billion people were eligible to vote, equivalent to 70 percent of the total population out of which 642 million voters participated in the election. Two major alliances- NDA(Nationsl Democratic Alliance) led by BJP and I.N.D.I.A.(Indian National Developmental Inclusive Alliance) comprised of 20+ parties competed against each other to lead the largest democracy of the world for the next 5 years. 

## Problem Statement: 

To conduct an extensive analysis of India's General Election Results to identify KPIs and other important insights by using SQL to retrieve the relevant information. 

The aim of the following analysis is to study the following parameters: 

    1) Total seats in the country and the total number of seats available for election in each seat. 

    2) Total seats won by NDA alliance as a whole and individual seats won by NDA alliance parties. 

    3) Total seats won by I.N.D.I.A alliance as a whole and individual seats won by I.N.D.I.A alliance parties.

    4) Which party alliance won the most seats(NDA, I.N.D.I.A or Others).
    
    5) To find out winning candidate's name, their party name, total votes and argin of victory for each constituency. 

    6) Distribution EVM votes v/s postal votes in each of 543 constituencies. 

    7)Which candidate won and which was the runner-up in each constituency.

    8) Which party won the most seats in a state and total number of seats won by each party. 

### Steps Followed: Data Preparation, Query & Manipulation Through SQL  

- Step 1 : Review the dataset which is available as a 5 csv files in excel and analyse the number of column heads and the number of rows for the data. Get familiar with the data and spend some time analysing the column heads along with data types.  

- Step 2: Load data into MS SQL server by creating a new database and importing the aforementioned flat files as tables. For each of the table inserted within the database, change the datatypes of the column heads as required.   For e.g. - 

      a) change the datatype of "S_N" column head in 'Constituency Details' table from tiny_int to int datatype. The data is then cleaned and transformed by checking for duplicates modifying the columns types appropriately for each of the column heads. 

      b) change the data type of 'Party' and 'Candidate Name' from nvarchar(50) to varchar(100) as the number of characters in these both column heads are quite long. 

      c) do remember to add primary keys to the tables while modifying the columns for each tables. 

- Step 3: Now, we will design a relationship between tables called "SCHEMA" in data-modelling where the 5 tables are related to atleast 1 other table through a common field. The constituencywise_results is the fact table and other 4 are dimension tables.
![Election 17](https://github.com/user-attachments/assets/d6d4fc2f-8289-4715-922e-b897f20a5ff8)
   
- Step 4: The next step includes firing SQL queries depending upon the problem statements and storing the results. We will also prepare a WORD file to document the SQL queries for future reference. 

#### SQL QUERIES: 

a) To find Total number of seats - 

    SELECT DISTINCT COUNT(Parliament_Constituency) As Total_Seats
    FROM constituencywise_results

![Election 1](https://github.com/user-attachments/assets/a65fd01a-a9f3-4cbf-b70d-5f6852ca1c09)

b) To find Total number of seats available for election in each state - 

    SELECT 
    s.State AS State_Name,
    COUNT(cr.Constituency_ID) AS Total_Seats_Available
    FROM 
    constituencywise_results cr
    JOIN 
    statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
    JOIN 
    states s ON sr.State_ID = s.State_ID
    GROUP BY s.State
    ORDER BY s.State;


 Below is the snapshot of the query fired and the subsequent result:
![Election 2](https://github.com/user-attachments/assets/2cb05d19-7891-46f3-b689-d1aa522604c2)   
    

c) To find total seats won by NDA alliance: (The NDA alliance parties can be searched on web) - 

    SELECT
    SUM(CASE 
    WHEN party IN(
    'Bharatiya Janata Party - BJP',
    'Telugu Desam - TDP',
    'Janata Dal  (United) - JD(U)',
    'Shiv Sena - SHS',
    'AJSU Party - AJSUP',
    'Apna Dal (Soneylal) - ADAL',
    'Asom Gana Parishad - AGP',
    'Hindustani Awam Morcha (Secular) - HAMS',
    'Janasena Party - JnP',
    'Janata Dal  (Secular) - JD(S)',
    'Lok Janshakti Party(Ram Vilas) - LJPRV',
    'Nationalist Congress Party - NCP',
    'Rashtriya Lok Dal - RLD',
    'Sikkim Krantikari Morcha - SKM')
    THEN [Won]
    ELSE 0
    END) AS NDA_Total_Seats_Won
    FROM partywise_results;
![Election 3](https://github.com/user-attachments/assets/47308e6e-cbc5-4bc2-af65-7f0c9b078c98)
    

 d) To find total seats won by NDA Alliance Parties  - 

    SELECT
    party AS Party_Name,
    won AS Seats_won
    FROM partywise_results
    WHERE party IN(
    'Bharatiya Janata Party - BJP',
    'Telugu Desam - TDP',
    'Janata Dal  (United) - JD(U)',
    'Shiv Sena - SHS',
    'AJSU Party - AJSUP',
    'Apna Dal (Soneylal) - ADAL',
    'Asom Gana Parishad - AGP',
    'Hindustani Awam Morcha (Secular) - HAMS',
    'Janasena Party - JnP',
    'Janata Dal  (Secular) - JD(S)',
    'Lok Janshakti Party(Ram Vilas) - LJPRV',
    'Nationalist Congress Party - NCP',
    'Rashtriya Lok Dal - RLD',
    'Sikkim Krantikari Morcha - SKM')

    ORDER BY Seats_won DESC;

The results of above query will be displayed as follows - 
![Election 4](https://github.com/user-attachments/assets/0bfa9f7c-7b29-41d2-ae79-80d1e9f79db8)

e) To find total seats won by I.N.D.I.A. alliance: (The INDIA alliance parties can be searched on web) -
    
    SELECT
    SUM(CASE 
    WHEN party IN(
    'Indian National Congress - INC',
    'Aam Aadmi Party - AAAP',
    'All India Trinamool Congress - AITC',
    'Bharat Adivasi Party - BHRTADVSIP',
    'Communist Party of India - CPI',
    'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
    'Communist Party of India  (Marxist) - CPI(M)',
    'Dravida Munnetra Kazhagam - DMK',
    'Indian Union Muslim League - IUML',
    'Jammu & Kashmir National Conference - JKN',
    'Jharkhand Mukti Morcha - JMM',
    'Kerala Congress - KEC',
    'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
    'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
    'Rashtriya Janata Dal - RJD',
    'Rashtriya Loktantrik Party - RLTP',
    'Revolutionary Socialist Party - RSP',
    'Samajwadi Party - SP',
    'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
    'Viduthalai Chiruthaigal Katchi - VCK')
    THEN [Won]
    ELSE 0
    END) AS NDA_Total_Seats_Won
    FROM partywise_results;

![Election 5](https://github.com/user-attachments/assets/9e0f0fc8-93ed-443a-8df7-ef3b381e9638)

f) To find total seats won by I.N.D.I.A. alliance parties - 

    SELECT
    party AS Party_Name,
    won AS Seats_won
    FROM partywise_results
    WHERE party IN(
    'Indian National Congress - INC',
    'Aam Aadmi Party - AAAP',
    'All India Trinamool Congress - AITC',
    'Bharat Adivasi Party - BHRTADVSIP',
    'Communist Party of India - CPI',
    'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
    'Communist Party of India  (Marxist) - CPI(M)',
    'Dravida Munnetra Kazhagam - DMK',
    'Indian Union Muslim League - IUML',
    'Jammu & Kashmir National Conference - JKN',
    'Jharkhand Mukti Morcha - JMM',
    'Kerala Congress - KEC',
    'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
    'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
    'Rashtriya Janata Dal - RJD',
    'Rashtriya Loktantrik Party - RLTP',
    'Revolutionary Socialist Party - RSP',
    'Samajwadi Party - SP',
    'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
    'Viduthalai Chiruthaigal Katchi - VCK')
    ORDER BY Seats_won DESC;

Below is the snapshot of the query fired:

![Election 6](https://github.com/user-attachments/assets/caa538bc-bee7-420f-ab96-9333d0875b01)

g) To segregate the parties into respective alliances: NDA, INDIA or Others - For this we will introduce a new column named "party_alliance" and use the following query - 

![Election 7](https://github.com/user-attachments/assets/2d047e4d-8c8d-4690-a2c9-aa92cd0bee0a)
The above message conveys that the query was successful and a new column was added. 


![Election 8](https://github.com/user-attachments/assets/ee2edeee-0411-4f21-ab67-abfd5cd653bd)

h) To find out which party alliance(NDA, INDIA or Others) won the most number of seats - 

      SELECT party_alliance,
      SUM(Won) AS Total_Seats_Won
      FROM partywise_results
      GROUP BY party_alliance
      ORDER BY Total_Seats_Won DESC;

![Election 9](https://github.com/user-attachments/assets/7afd8305-f105-4166-9997-6527726fec15)    

i) To find out winning candidate’s name, their party name, total votes, and margin of victory for a specific state and constituency - 

    SELECT
    cr.Winning_Candidate,
    pr.Party,
    cr.Total_Votes,
    cr.Margin,
    s.State, 
    cr.Constituency_Name
    FROM constituencywise_results cr INNER JOIN partywise_results pr ON cr.Party_ID = pr.Party_ID
    INNER JOIN statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
    INNER JOIN states s ON sr.State_ID = s.State_ID
    WHERE cr.Constituency_Name = 'LUDHIANA'


![Election 10](https://github.com/user-attachments/assets/f18ab5fe-25ab-4288-bb12-38d8926b6ffc)

j) To find out the distribution of EVM votes vs Postal votes for candidates in a specific constituency – 

    SELECT
    cd.Candidate,
    cr.Constituency_Name,
    cd.EVM_Votes,
    cd.Postal_Votes,
    cd.Total_Votes
    FROM constituencywise_results cr INNER JOIN constituencywise_details cd
    ON cr.Constituency_ID = cd.Constituency_ID
    WHERE cr.Constituency_Name = 'LUDHIANA';

![Election 11](https://github.com/user-attachments/assets/8a85fc5d-e668-42c7-8940-fe9ab83b6642)

k) Next step is calculating the daily sales for a selected month using the below query – 

    SELECT 
    p.Party,
    COUNT(cr.Constituency_ID) AS Seats_Won
    FROM 
    constituencywise_results cr
    INNER JOIN 
    partywise_results p ON cr.Party_ID = p.Party_ID
    INNER JOIN 
    statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
    INNER JOIN states s ON sr.State_ID = s.State_ID
    WHERE 
    s.state = 'Punjab'
    GROUP BY 
    p.Party
    ORDER BY 
    Seats_Won DESC;
![Election 12](https://github.com/user-attachments/assets/b7f17b24-d8ce-4a10-94ec-a1cd3ff2c1b6)
      

l) To find out the total number of seats won by each party Alliance(NDA, INDIA, or Others) in each state - 

    SELECT 
    s.State AS State_Name,
    SUM(CASE WHEN p.party_alliance = 'NDA' THEN 1 ELSE 0 END) AS NDA_Seats_Won,
	SUM(CASE WHEN p.party_alliance = 'INDIA' THEN 1 ELSE 0 END) AS INDIA_Seats_Won,
	SUM(CASE WHEN p.party_alliance = 'Others' THEN 1 ELSE 0 END) AS Others_Seats_Won
    FROM 
    constituencywise_results cr
    JOIN 
    partywise_results p ON cr.Party_ID = p.Party_ID
    JOIN 
    statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
    JOIN states s ON sr.State_ID = s.State_ID
    WHERE 
    p.party_alliance IN ('NDA', 'INDIA', 'Others')
    GROUP BY 
    s.State
    ORDER BY 
    s.State;

![Election 13](https://github.com/user-attachments/assets/4b2b1e72-c3d0-4ba0-8131-7b0239940891)

m) To find out the Top 10 candidates who received maximum number of EVM votes along with their constituency name - 

      SELECT TOP 10
    cr.Constituency_Name,
	cd.Constituency_ID,
	cd.Candidate,
	cd.EVM_Votes
	FROM constituencywise_details cd
	INNER JOIN constituencywise_results cr ON cd.Constituency_ID = cr.Constituency_ID
	WHERE
	cd.EVM_Votes = (
	SELECT MAX(cd1.EVM_Votes)
	FROM constituencywise_details cd1
	WHERE cd1.Constituency_ID = cd.Constituency_ID
	)
	ORDER BY cd.EVM_Votes DESC;

![Election 14](https://github.com/user-attachments/assets/c1f34501-a757-4c7a-9a6b-b2b13edec2cd)

n) The query for examining the Top 10 Product Type is fired as follows - 


    WITH RankedCandidates AS(
    SELECT
    cd.Constituency_ID,
    cd.Candidate,
    cd.Party,
    cd.EVM_Votes,
    cd.Postal_Votes,
    cd.EVM_Votes + cd.Postal_Votes AS Total_Votes,
    ROW_NUMBER() OVER(PARTITION BY cd.Constituency_ID ORDER BY cd.EVM_Votes + cd.Postal_Votes DESC) AS VoteRank
    FROM 
        constituencywise_details cd
    INNER JOIN 
        constituencywise_results cr ON cd.Constituency_ID = cr.Constituency_ID
    INNER JOIN 
        statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
    INNER JOIN 
        states s ON sr.State_ID = s.State_ID
    WHERE 
        s.State = 'Punjab'
)

    SELECT 
    cr.Constituency_Name,
    MAX(CASE WHEN rc.VoteRank = 1 THEN rc.Candidate END) AS Winning_Candidate,
    MAX(CASE WHEN rc.VoteRank = 2 THEN rc.Candidate END) AS Runnerup_Candidate
    FROM 
    RankedCandidates rc
    JOIN 
    constituencywise_results cr ON rc.Constituency_ID = cr.Constituency_ID
    GROUP BY 
    cr.Constituency_Name
    ORDER BY 
    cr.Constituency_Name;

![Election 15](https://github.com/user-attachments/assets/dd26f284-ad16-4efe-a6e4-5eaca4236dd8)

o) To find out what are the total number of seats, total candidates, total parties,  total EVM votes, total Postal votes, total votes(EVM + Postal) for a particular state - 
 - 

    SELECT
    COUNT( DISTINCT cd.Candidate) AS Total_Candidates,
    COUNT(DISTINCT cr.Parliament_Constituency) AS Total_Seats,
    COUNT(DISTINCT cd.Party) AS Total_Parties,
    SUM(cd.EVM_Votes + cd.Postal_Votes) AS Total_Votes,
    SUM(cd.EVM_Votes) AS Total_EVM_Votes,
    SUM(cd.Postal_Votes) AS Total_Postal_Votes

    FROM
    constituencywise_results cr 
    INNER JOIN constituencywise_details cd ON cr.Constituency_ID = cd.Constituency_ID
    INNER JOIN statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
    INNER JOIN states s ON sr.State_ID = s.State_ID
    INNER JOIN partywise_results p ON cr.Party_ID = p.Party_ID

    WHERE s.State = 'Punjab';


![Election 16](https://github.com/user-attachments/assets/5dd3ec52-ebbf-431a-8766-cc5c1b2e7820)       

