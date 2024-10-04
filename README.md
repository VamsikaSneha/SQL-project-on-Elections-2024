# Iɴᴅɪᴀ Gᴇɴᴇʀᴀʟ Eʟᴇᴄᴛɪᴏɴs 2024 Dᴀᴛᴀ Aɴᴀʟʏsɪs Usɪɴɢ SQL

![](https://www.financialexpress.com/wp-content/uploads/2023/09/LOK-SABHA-ELECTION-2024.jpg)

## Overview 

This project involves an in-depth analysis of the 2024 India General Elections results using SQL. The aim is to extract insights related to the election results across different constituencies and states. The project addresses key business questions such as the distribution of seats won by political alliances, vote margins, and the performance of political parties.

##  Objectives

* Analyze the number of seats won by political alliances (NDA, I.N.D.I.A, OTHER).
* Identify the top candidates based on EVM votes and the closest races.
* Study content distribution based on geographic and demographic factors.
* Analyze party-wise and state-wise election outcomes.

##  Business Problems and Solutions
### Total Seats
```sql
SELECT 
    type,
    COUNT(*)
FROM netflix
GROUP BY 1;
```
### What is the total number of seats available for elections in each state
```sql
SELECT 
    s.State AS State_Name,
    COUNT(cr.Constituency_ID) AS Total_Seats_Available
FROM 
    constituencywise_results cr
INNER JOIN 
    statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
INNER JOIN 
    states s ON sr.State_ID = s.State_ID
GROUP BY 
    s.State
ORDER BY 
    s.State;
```
### Total Seats Won by NDA Allianz
```sql
SELECT 
    SUM(CASE 
            WHEN party IN (
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
                'Sikkim Krantikari Morcha - SKM'
            ) THEN [Won]
            ELSE 0 
        END) AS NDA_Total_Seats_Won
FROM 
    partywise_results
```
### Seats Won by NDA Allianz Parties
```sql
SELECT 
    party as Party_Name,
    won as Seats_Won
FROM 
    partywise_results
WHERE 
    party IN (
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
        'Sikkim Krantikari Morcha - SKM'
    )
ORDER BY Seats_Won DESC;
```
### Total Seats Won by I.N.D.I.A. Allianz
```sql
SELECT 
    SUM(CASE 
            WHEN party IN (
                'Indian National Congress - INC',
                'Aam Aadmi Party - AAAP',
                'All India Trinamool Congress - AITC',
                'Bharat Adivasi Party - BHRTADVSIP',
                'Communist Party of India  (Marxist) - CPI(M)',
                'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
                'Communist Party of India - CPI',
                'Dravida Munnetra Kazhagam - DMK',
                'Indian Union Muslim League - IUML',
                'Nat`Jammu & Kashmir National Conference - JKN',
                'Jharkhand Mukti Morcha - JMM',
                'Jammu & Kashmir National Conference - JKN',
                'Kerala Congress - KEC',
                'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
                'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
                'Rashtriya Janata Dal - RJD',
                'Rashtriya Loktantrik Party - RLTP',
                'Revolutionary Socialist Party - RSP',
                'Samajwadi Party - SP',
                'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
                'Viduthalai Chiruthaigal Katchi - VCK'
            ) THEN [Won]
            ELSE 0 
        END) AS INDIA_Total_Seats_Won
FROM 
    partywise_results
```
### Seats Won by I.N.D.I.A. Allianz Parties
```sql
SELECT 
    party as Party_Name,
    won as Seats_Won
FROM 
    partywise_results
WHERE 
    party IN (
        'Indian National Congress - INC',
                'Aam Aadmi Party - AAAP',
                'All India Trinamool Congress - AITC',
				'Bharat Adivasi Party - BHRTADVSIP',
                'Communist Party of India  (Marxist) - CPI(M)',
                'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
                'Communist Party of India - CPI',
                'Dravida Munnetra Kazhagam - DMK',
                'Indian Union Muslim League - IUML',
                'Nat`Jammu & Kashmir National Conference - JKN',
                'Jharkhand Mukti Morcha - JMM',
                'Jammu & Kashmir National Conference - JKN',
                'Kerala Congress - KEC',
                'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
                'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
                'Rashtriya Janata Dal - RJD',
                'Rashtriya Loktantrik Party - RLTP',
                'Revolutionary Socialist Party - RSP',
                'Samajwadi Party - SP',
                'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
                'Viduthalai Chiruthaigal Katchi - VCK'
    )
ORDER BY Seats_Won DESC;
```

### Add new column field in table partywise_results to get the Party Allianz as NDA, I.N.D.I.A and OTHER
```sql
ALTER TABLE partywise_results
ADD party_alliance VARCHAR(50);
```
## I.N.D.I.A Allianz
```sql
UPDATE partywise_results
SET party_alliance = 'I.N.D.I.A'
WHERE party IN (
    'Indian National Congress - INC',
    'Aam Aadmi Party - AAAP',
    'All India Trinamool Congress - AITC',
    'Bharat Adivasi Party - BHRTADVSIP',
    'Communist Party of India  (Marxist) - CPI(M)',
    'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
    'Communist Party of India - CPI',
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
    'Viduthalai Chiruthaigal Katchi - VCK'
);
```
## NDA Allianz
```sql
UPDATE partywise_results
SET party_alliance = 'NDA'
WHERE party IN (
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
    'Sikkim Krantikari Morcha - SKM'
);
```
## OTHER
```sql
UPDATE partywise_results
SET party_alliance = 'OTHER'
WHERE party_alliance IS NULL;
``` 
### Which party alliance (NDA, I.N.D.I.A, or OTHER) won the most seats across all states?
```sql
SELECT 
    p.party_alliance,
    COUNT(cr.Constituency_ID) AS Seats_Won
FROM 
    constituencywise_results cr
JOIN 
    partywise_results p ON cr.Party_ID = p.Party_ID
WHERE 
    p.party_alliance IN ('NDA', 'I.N.D.I.A', 'OTHER')
GROUP BY 
    p.party_alliance
ORDER BY 
    Seats_Won DESC;
``` 
### Winning candidate's name, their party name, total votes, and the margin of victory for a specific state and constituency?
```sql
SELECT cr.Winning_Candidate, p.Party, p.party_alliance, cr.Total_Votes, cr.Margin, cr.Constituency_Name, s.State
FROM constituencywise_results cr
JOIN partywise_results p ON cr.Party_ID = p.Party_ID
JOIN statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
JOIN states s ON sr.State_ID = s.State_ID
WHERE s.State = 'Andhra Pradesh' AND cr.Constituency_Name = 'AMALAPURAM(SC)';
```
### What is the distribution of EVM votes versus postal votes for candidates in a specific constituency?
```sql
SELECT 
    cd.Candidate,
    cd.Party,
    cd.EVM_Votes,
    cd.Postal_Votes,
    cd.Total_Votes,
    cr.Constituency_Name
FROM 
    constituencywise_details cd
JOIN 
    constituencywise_results cr ON cd.Constituency_ID = cr.Constituency_ID
WHERE 
    cr.Constituency_Name = 'AMALAPURAM(SC)'
ORDER BY cd.Total_Votes DESC;
```
### Which parties won the most seats in s State, and how many seats did each party win?
```sql
SELECT 
    p.Party,
    COUNT(cr.Constituency_ID) AS Seats_Won
FROM 
    constituencywise_results cr
JOIN 
    partywise_results p ON cr.Party_ID = p.Party_ID
JOIN 
    statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
JOIN states s ON sr.State_ID = s.State_ID
WHERE 
    s.state = 'Andhra Pradesh'
GROUP BY 
    p.Party
ORDER BY 
    Seats_Won DESC;
```
### What is the total number of seats won by each party alliance (NDA, I.N.D.I.A, and OTHER) in each state for the India Elections 2024
```sql
SELECT 
    s.State AS State_Name,
    SUM(CASE WHEN p.party_alliance = 'NDA' THEN 1 ELSE 0 END) AS NDA_Seats_Won,
    SUM(CASE WHEN p.party_alliance = 'I.N.D.I.A' THEN 1 ELSE 0 END) AS INDIA_Seats_Won,
	SUM(CASE WHEN p.party_alliance = 'OTHER' THEN 1 ELSE 0 END) AS OTHER_Seats_Won
FROM 
    constituencywise_results cr
    INNER JOIN 
    partywise_results p ON cr.Party_ID = p.Party_ID
 INNER JOIN 
    statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
 INNER JOIN 
    states s ON sr.State_ID = s.State_ID
WHERE 
    p.party_alliance IN ('NDA', 'I.N.D.I.A',  'OTHER')  -- Filter for NDA and INDIA alliances
GROUP BY 
    s.State
ORDER BY 
    s.State;
```
### Which candidate received the highest number of EVM votes in each constituency (Top 10)?
```sql
SELECT TOP 10
    cr.Constituency_Name,
    cd.Constituency_ID,
    cd.Candidate,
    cd.EVM_Votes
FROM 
    constituencywise_details cd
JOIN 
    constituencywise_results cr ON cd.Constituency_ID = cr.Constituency_ID
WHERE 
    cd.EVM_Votes = (
        SELECT MAX(cd1.EVM_Votes)
        FROM constituencywise_details cd1
        WHERE cd1.Constituency_ID = cd.Constituency_ID
    )
ORDER BY 
    cd.EVM_Votes DESC;
```
### Which candidate won and which candidate was the runner-up in each constituency of State for the 2024 elections?
```sql
WITH RankedCandidates AS (
    SELECT 
        cd.Constituency_ID,
        cd.Candidate,
        cd.Party,
        cd.EVM_Votes,
        cd.Postal_Votes,
        cd.EVM_Votes + cd.Postal_Votes AS Total_Votes,
        ROW_NUMBER() OVER (PARTITION BY cd.Constituency_ID ORDER BY cd.EVM_Votes + cd.Postal_Votes DESC) AS VoteRank
    FROM 
        constituencywise_details cd
    JOIN 
        constituencywise_results cr ON cd.Constituency_ID = cr.Constituency_ID
    JOIN 
        statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
    JOIN 
        states s ON sr.State_ID = s.State_ID
    WHERE 
        s.State = 'Andhra Pradesh'
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
```
### For the state of Andhra Pradesh, what are the total number of seats, total number of candidates, total number of parties, total votes (including EVM and postal), and the breakdown of EVM and postal votes?
```sql
SELECT 

    COUNT(DISTINCT cr.Constituency_ID) AS Total_Seats,
    COUNT(DISTINCT cd.Candidate) AS Total_Candidates,
    COUNT(DISTINCT p.Party) AS Total_Parties,
    SUM(cd.EVM_Votes + cd.Postal_Votes) AS Total_Votes,
    SUM(cd.EVM_Votes) AS Total_EVM_Votes,
    SUM(cd.Postal_Votes) AS Total_Postal_Votes
FROM 
    constituencywise_results cr
JOIN 
    constituencywise_details cd ON cr.Constituency_ID = cd.Constituency_ID
JOIN 
    statewise_results sr ON cr.Parliament_Constituency = sr.Parliament_Constituency
JOIN 
    states s ON sr.State_ID = s.State_ID
JOIN 
    partywise_results p ON cr.Party_ID = p.Party_ID
WHERE 
    s.State = 'Andhra Pradesh';
```
## Findings and Conclusions


1. 𝑻𝙤𝒕𝙖𝒍 𝑺𝙚𝒂𝙩𝒔 𝑫𝙞𝒔𝙩𝒓𝙞𝒃𝙪𝒕𝙞𝒐𝙣:

The election results show the distribution of seats across different states. Using SQL queries, we determined that the total number of seats varies significantly from state to state, with some states having more electoral weight.

2. 𝑨𝙡𝒍𝙞𝒂𝙣𝒄𝙚-𝙒𝒊𝙨𝒆 𝑷𝙚𝒓𝙛𝒐𝙧𝒎𝙖𝒏𝙘𝒆:

𝑁𝐷𝐴 𝐴𝑙𝑙𝑖𝑎𝑛𝑐𝑒: The National Democratic Alliance (NDA) performed well across several states, winning a significant number of seats. Major contributors to NDA's success include parties like Bharatiya Janata Party (BJP), Telugu Desam (TDP), and Janata Dal (United) - JD(U).

𝐼.𝑁.𝐷.𝐼.𝐴. 𝐴𝑙𝑙𝑖𝑎𝑛𝑐𝑒: The I.N.D.I.Aalliance also secured a considerable number of seats, with strong representation from Indian National Congress (INC), Aam Aadmi Party (AAP), and Dravida Munnetra Kazhagam (DMK).

𝑂𝑡ℎ𝑒𝑟 𝑃𝑎𝑟𝑡𝑖𝑒𝑠: There were also other regional parties that performed independently of the two major alliances, winning seats and contributing to a more diverse political landscape.

3. 𝑻𝙤𝒑 𝑪𝙖𝒏𝙙𝒊𝙙𝒂𝙩𝒆𝙨 𝙖𝒏𝙙 𝘾𝒍𝙤𝒔𝙚𝒔𝙩 𝙍𝒂𝙘𝒆𝙨:

The analysis revealed the top candidates based on EVM votes. The constituencies with the closest margins of victory were highlighted, which indicates highly competitive races in specific regions. For example, some constituencies in Andhra Pradesh showed narrow victory margins.

4. 𝑺𝙩𝒂𝙩𝒆-𝑾𝙞𝒔𝙚 𝙎𝒆𝙖𝒕 𝑫𝙞𝒔𝙩𝒓𝙞𝒃𝙪𝒕𝙞𝒐𝙣:

In states like Andhra Pradesh, Telangana, and Uttar Pradesh, the competition between NDA, I.N.D.I.A., and other parties was particularly intense. SQL queries helped break down the seats won by each alliance, giving insight into regional dynamics.

5. 𝑽𝙤𝒕𝙚 𝘽𝒓𝙚𝒂𝙠𝒅𝙤𝒘𝙣:

A detailed analysis of the EVM vs Postal Votes showed how different voting methods contributed to the final results. In certain constituencies, postal votes had a significant impact on the final outcome.

6. 𝑹𝙚𝒈𝙞𝒐𝙣𝒂𝙡 𝙖𝒏𝙙 𝙉𝒂𝙩𝒊𝙤𝒏𝙖𝒍 𝑻𝙧𝒆𝙣𝒅𝙨: 

The data shows that the NDA alliance won the most seats across several states, but I.N.D.I.A. alliance parties dominated in others. This reflects the complex and diverse political preferences across the country.

7. 𝙋𝒂𝙧𝒕𝙮 𝙖𝒏𝙙 𝘼𝒍𝙡𝒊𝙖𝒏𝙘𝒆 𝑻𝙧𝒆𝙣𝒅𝙨:

SQL analysis helped track the number of seats won by individual parties and alliances, providing a clear view of their relative strengths and areas of dominance.

8. 𝑪𝙖𝒏𝙙𝒊𝙙𝒂𝙩𝒆-𝑳𝙚𝒗𝙚𝒍 𝑨𝙣𝒂𝙡𝒚𝙨𝒊𝙨:

The analysis also drilled down to the candidate level, identifying both winners and runners-up across constituencies. This provided detailed insights into the competition and voter preferences.

9. 𝙄𝒏𝙨𝒊𝙜𝒉𝙩𝒔 𝒐𝙣 𝙋𝒐𝙡𝒊𝙩𝒊𝙘𝒂𝙡 𝘽𝒂𝙩𝒕𝙡𝒆𝙜𝒓𝙤𝒖𝙣𝒅𝙨:

The project identified key political battlegrounds where the race was extremely tight. These insights are crucial for predicting future election strategies for parties and alliances.

𝑪𝒐𝒏𝒄𝒍𝒖𝒔𝒊𝒐𝒏:

𝘛𝘩𝘪𝘴 𝘥𝘢𝘵𝘢 𝘢𝘯𝘢𝘭𝘺𝘴𝘪𝘴 𝘱𝘳𝘰𝘷𝘪𝘥𝘦𝘴 𝘢 𝘤𝘰𝘮𝘱𝘳𝘦𝘩𝘦𝘯𝘴𝘪𝘷𝘦 𝘷𝘪𝘦𝘸 𝘰𝘧 𝘵𝘩𝘦 𝘐𝘯𝘥𝘪𝘢 𝘎𝘦𝘯𝘦𝘳𝘢𝘭 𝘌𝘭𝘦𝘤𝘵𝘪𝘰𝘯𝘴 2024, 𝘰𝘧𝘧𝘦𝘳𝘪𝘯𝘨 𝘪𝘯𝘴𝘪𝘨𝘩𝘵𝘴 𝘪𝘯𝘵𝘰 𝘱𝘰𝘭𝘪𝘵𝘪𝘤𝘢𝘭 𝘢𝘭𝘭𝘪𝘢𝘯𝘤𝘦𝘴, 𝘤𝘢𝘯𝘥𝘪𝘥𝘢𝘵𝘦 𝘱𝘦𝘳𝘧𝘰𝘳𝘮𝘢𝘯𝘤𝘦𝘴, 𝘢𝘯𝘥 𝘳𝘦𝘨𝘪𝘰𝘯𝘢𝘭 𝘷𝘰𝘵𝘪𝘯𝘨 𝘵𝘳𝘦𝘯𝘥𝘴. 𝘛𝘩𝘦 𝘚𝘘𝘓-𝘣𝘢𝘴𝘦𝘥 𝘢𝘯𝘢𝘭𝘺𝘴𝘪𝘴 𝘩𝘦𝘭𝘱𝘴 𝘱𝘰𝘭𝘪𝘤𝘺𝘮𝘢𝘬𝘦𝘳𝘴, 𝘱𝘰𝘭𝘪𝘵𝘪𝘤𝘢𝘭 𝘢𝘯𝘢𝘭𝘺𝘴𝘵𝘴, 𝘢𝘯𝘥 𝘳𝘦𝘴𝘦𝘢𝘳𝘤𝘩𝘦𝘳𝘴 𝘶𝘯𝘥𝘦𝘳𝘴𝘵𝘢𝘯𝘥 𝘵𝘩𝘦 𝘤𝘰𝘮𝘱𝘭𝘦𝘹 𝘥𝘺𝘯𝘢𝘮𝘪𝘤𝘴 𝘰𝘧 𝘐𝘯𝘥𝘪𝘢𝘯 𝘱𝘰𝘭𝘪𝘵𝘪𝘤𝘴 𝘢𝘯𝘥 𝘦𝘭𝘦𝘤𝘵𝘪𝘰𝘯𝘴, 𝘢𝘯𝘥 𝘪𝘵 𝘤𝘰𝘶𝘭𝘥 𝘴𝘦𝘳𝘷𝘦 𝘢𝘴 𝘢 𝘣𝘢𝘴𝘦 𝘧𝘰𝘳 𝘱𝘳𝘦𝘥𝘪𝘤𝘵𝘪𝘯𝘨 𝘧𝘶𝘵𝘶𝘳𝘦 𝘵𝘳𝘦𝘯𝘥𝘴.
