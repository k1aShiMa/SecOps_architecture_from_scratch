# SecOps Architecture from Scratch

We'll use docker for hosting these like containers lol

Then I guess i'll use dunno MITRE ATT&CK for TTP purposes, then you can check it in the MITRE map or something

# Used TTPs

* T1589 - Recon Phase: Gather Victim Identity Information through social media like linkedin or the dunno list of names
* T1583.001 - Resource Development: Domestic Domains We'll use like a fake domain, y'know like not egyetem.com it's like dunno egyetem1.com
* T1566.002 - Initial Access: Like we'll get a grip on the users creds or something, it's like a spearphishing attack in this case
* T1557.002 - Credential Access: AiTM like Adversary in the middle attack type, if you don't know it just look it up with your favourite search engine XDDD

# Just you know, i'm doing this 'cuz i'm bored like hell, and it's already in my portfolio so ye... it's a kinda similar thing what i had for a time but i never published it to github cuz it didn't wanna work

Okay so now we've got the grip what we'll do and ye, it'll be a rollercoaster i guess but hell ye

# Technologies Used:

* TheHive - Case management and alerting system xd
* ELK - for SIEM dashboard and ye
* n8n - for log shipping
* Cassandra - DB for TheHive
* Evilginx - For phislets and session hijack lol
* KasmWeb - Victim Machine