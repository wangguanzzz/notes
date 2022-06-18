1. SRE youtube
https://www.youtube.com/watch?v=OnK4IKgLl24
* what makes system unreliable
  - chnages in the infra
  - changes in the platform (k8s)
  - changes in service & application

* SLA, error rate( 99%SLA 100 in 10000 request can failed)
* Error budget : allowed downtime of a service

SRE tasks and responsibilities:
1. automation - create automated processes for operational aspects
2. monitoring & logging ( observability for system performance)
3. configure monitoring, logging & alerting
4. develop custom service to achieve this
5. on-call support
6. do postmortem
key -> identify the issue ASAP
complexity: system is high availability, repliablility, self healing,  sometimes multiple things go wrong ( so called chain of issues)

difference between SRE and Devops
* devops is automated streamline relase process (more focus on CI/CD), release fast, release quality code, SRE more focus on reliability


* role  : dev -> make new releases   ops-> long list of checking
  SRE -> tries to automate the process of evaluating the affects the changes will have.  to make the change fast and safe



2. after issue analysis template
* what happened
* what we learned
* how we fixed it

postmortems



troubleshooting best practice
* sympton
* check the time error occur, log checking/ check the changes
* get assumed reason
* re-produce