#nsqlookupd migrate proxy
Migrate proxy to help migrate from old nsq to youzan nsq.
 
Migrate works as proxy of nsqlookupd of nsq and youzan nsq. It delegates lookup requests to nsqlookupd in old and new, 
migrate response and return to nsq client, according to migrate status read from DCC.

Migrate status defines as follow:
<pre>const (
     	M_OFF = iota    //0. migrate not start
     	M_CSR           //1. return old&new lookup response for consumer, and old lookup response for producer
     	M_CSR_PDR       //2. return old&nsq lookup response for consumer, and new lookup response for producer
     	M_FIN           //3. return new lookup response for consumer&producer
     )</pre>
     
>Note: migrate from youzan nsq to another does not support, since partition info in two youzan nsq cold not mix.

##Deploy
start with config file:
<pre>nsqlookup_migrate -config=conf/nsqlookup_migrate.conf</pre>

##Config
properties defined in nsqlookup migrate:
<pre>
origin-lookupd-http = "http://origin.nsqlookupd:4161"
target-lookupd-http = "http://127.0.0.1:4161"
dcc-url = "http://10.9.7.75:8089"
dcc-backup-file = "/tmp/nsqlookupd_migrate/backup"
env = "qa"
log-level = 2
log-dir = "/data/logs/nsqlookup_migrate"
migrate-dcc-key = ""

//following config applies for migrate proxy tester, including tester accesses nsqlookup proxy and tester modifies migrate configs
test=true
http-address-test = "http://10.9.152.26:4171"
test-client-num = 100
mc-test = true
</pre>