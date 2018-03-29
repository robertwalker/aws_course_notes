Overview
--------

IPv6 is now supported since sometime in 2016

### Exam Tips

* ELBs don't have pre-defined IPv4 addresses, you resolve them using DNS name
* Understand the difference between an Alias Record and a CNAME
* Given the choice, allways choose and Alias Recored over a CNAME

Route53
-------

Route53 is a Global service

### Routing Policies

* Simple
* Weighted
* Latency
* Failover
* Geolocation

### Simple

Defult policy when creating new record set. Most used with single web server.

### Weighted

Allows traffic to be split based on different weights assigned. For example 10% sent to us-east-1 and 90% to eu-west-1.

### Latency

Routes traffic based on lowest network latency for the end user.

### Failover

Used for creating an active/passive setup. Will sent traffic to active as long as it is available and will failover to passive in case active fails.

Utializes health check information.

### Geolocation

Traffic is sent based on the geographic locaition of end users. Used to keep traffic routed to chosen locations. Might be used for resouce localization or possibly polictial limitations.

### Exam Tips

* ELBs never have a physical IP address. Only uses DNS names
* Understand the difference between Alias Record and CNAME
* Given the choice, always choose Alias Record over CNAME
* Remember the routing policies and their use cases