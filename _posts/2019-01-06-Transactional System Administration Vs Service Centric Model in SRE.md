---
layout:            post
title:             "Transactional System Administration Vs Service Centric Model in SRE"
date:              2019-01-06 20:10:00 +0300
tags:              TransactionalAdministrationSystem + ServiceCentricModel
category:          Tutorial
author:            Poonam Agarwal
---
### What is Transactional System Administration?
	Think of it as a scenario between Customer and SysAdmin:
	- Customer : Would you do xyz task?
	- SysAdmin : Yes, Done!
	- Customer verifies. 
	This is a typical transaction system. The best example is ticket system.

### Why is it Bad?
	- It is a reactive process as the SysAdmin has to do job in a limited time frame which creates pressure over them.
	- It discourages the long term planning, for eg. Nobody comes and says I am working on service and will need this machine configuration in future. As a result there are emergency hacks and outages. Operations is an afterthought.
	- No automation for optimization and prioritization of the requests.
	- Customer considers SRE as "servant" and SRE thinks customer as "ever demanding child". Both of these is bad.
	- There is only one way to scale i.e. hire more people!
	- Users hate to open tickets for every task and hang on it.
	The whole ticket system is creating a pressured environment for SREs , lets call it as "Push" mechanism is working here.

### How did we reach here?
	Have we really, ever gave a thought that do we really need Ticket System?  
	Actually No! It's not required.
	But then you need a way to track your work, right!

### Some of the ways that different organizations has adopted.
#### 1) Baseline- Convert "Push" -> "Collaboration"
	How? There are 2 ways: 
	- Put one SRE member in different development team's Agile meetings.
	- Put each team lead of different development team's in SRE Agile meetings.
		> Another version could be SRE communicating with all stakeholders of the product not just developers.
	Both of the above points gives a single solution by solving Administartion related problems in Dev teams. Isn't it? 
	SRE memebers become aware of their problems in the meetings and can solve them.
	Dev teams can also use SRE chat rooms for their issues.

#### 2) Baseline- Convert "Push" -> "Automations"
	Some eg:
	- Automating compile and build in CI/CD for developers to do on their own. Click or give some APIs and it's done.
	- Auotmate load balance to point at service replicas.
	- Automate VM creation.
	- Laptop distribution: For new employees we should maintain laptop distribution system as-
			> SRE tracks HR DB for new employee, Emails when eligible and creates a portal for ordering machines.
			> Automates OS installation for use by laptop delivery crew.
			> Does capacity planning generates purchase orders etc for purchasing and finance group.
	Here we try to automate as much as possible for users to get the task done by automations instead of killing SRE's time. This also keeps motivation of SRE high.

#### 3) Baseline- Convert "Push" -> "Pull"
	Use Kanban boards, some good eg are:
	- Trello board
	- Learnkit.com

<div><figure><img src="{{ site.github.url }}/media/img/kanban-board.png" /><figcaption></figcaption></figure></div>

	- Assign 3 task per person in action per week. You pull 3 tasks through system per week.
	This way everyone could prioritize their tasks. It's upto the product management to decide in case of high loads whether prioritize the tasks or hire more people.


Some References:
<a href="https://www.usenix.org/conference/lisa15/conference-program/presentation/limoncelli">
Reference
</a>