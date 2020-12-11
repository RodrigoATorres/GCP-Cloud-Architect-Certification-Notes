Google Cloud Platform 101 

https://www.youtube.com/watch?v=trJaoEtBh6w&t=15s

# Lift and Shift
Getting a ready application outside GC and bring to it
- Create instance

- Cloud Storage
	- Options:
		- Selectivly make content available to public
	- Types:
	
	|<br>|Multi-regional|Regional|Nearline|Coldline|
	|-|-|-|-|-|
	|Price| 0.026 US$/GB/month|0.02 US$/GB/month|0.01 US$/GB/month|0.007 US$/GB/month|
	|Access Price|free|free|0.01 US$/GB|0.05 US$/GB|
	|Uses|Global<br>Web accessible|Regional<br>Big Data Jobs<br>Inteanally processed|Backups<br>Long tail media|Compliance<br>Disaster recovery|
<br>

- Cloud Database
	- SQLS
	
	|<br>|Cloud SQL|Cloud Spanner|
	|-|-|-|-|-|
	|Scalabilty|Vertical|Horizontal|
	|Features|Managed backups<br>Easy to setup replicas<br> MySQL and Postgres|Structured Data<br>Highly Available<br>Strongly consistent<br>Globally distributed<br>Cost effective at scale<br>|
	|Uses|Traditional SQL|Very large apps|
	
	- NoSQLS
	
	|<br>|Cloud Datastore|Cloud Firestore| Cloud Bigtable
	|-|-|-|-|-|
	|Features|Document base<br>Indexable<br>Giant|Document base<br>Indexable<br>Good mobile and web SDK|Columnar<br>LOW latency<br>Hbase API compatible<br>Cost effetive at scale|
	|Uses|Communication through backend|Aplications where the frontend communicates with DB directly |

- Data Tools
	- Pub/sub
		- similar to mqtt!? 

- Instance Gorups
	- Managed or unmanaged
	- autoscale
	- autoheal

- Networking
	- HTTP(S)
	- TCP
	- UDP
	- Internal Only (accessable within project)
- Container products
	- Kuvernetes ENgine
		- Managed Kubernetes (containers as a service)
		- Smart defaults for
			- Monitoring, logging, dns, service discovery, auto scaling
	- App engine flexible
		- Custom runtime
			- Own dockerfile
		- Autoscaling
		- One endpoint per service
- Serverless
	- pay as you go
	- Cloud Function
		- function based
		- Managed node.js
		- Event driven
	- App Engine standart
		- Specific languages (python2.7, java7+8, php5.5, go)
		- Constrained capabilities
		- Incredibly rapid scale