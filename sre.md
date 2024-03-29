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


SRE three functions:
- define availability
  => SLO: service level objective
- determine level of availability
  => SLI: service level indicator
- detail what happens when availability fails
  => SLA: service level agreement


## SLI
SLI => **quantified reliability**
sli are metrics over time - specific to a user journey such as request/response, data processing, or storage
typical
- request latency
- failure rate
- batch throughput

google 4 golden signals
- latency
- errors
- traffic ( how much demand is directed at your service )
- saturation ( a meausre of how close to fully utilized services' resources)

1. limit number of SLIs
  3-5 per user journey
2. reduce complexity
3. prioritize journeys
4. aggregate similiar SLIs
5. bucket to disinguish response class
6. collect data at load balancer

## SLO

SLO are tied to your SLIs
- measured by SLI
- can be a single target or range of targets
- SLI <= SLO
  OR
  ( lower bound <= SLI <= upper bound) = SLO
- common SLOs: 99.5%, 99.9% (three nines) , 99.99% ( four nines)

SLI  => home page latency requests < 300 ms  over last 5 minutes @ 95% percentile
SLO: 95% percentile homepage SLI will succed 99.9% over next year

make your SLO achievable
(1) base on past performance
(2) in no historical data , collect some