# mas_tuning
MAS Performance Tuning Suggestions
```bash
1.Disable alwaysCheckDatabaseOnExecute
EnvironmentManager -> Configuration -> Micro Analytic Score service -> sas.microanalyticservice.system -> supplementalProperties
service.alwayscheckdatabaseonexecute: false

2.Disable Eventing
EnvironmentManager -> Configuration -> Micro Analytic Score service -> JVM
java_option_auth: -Dsas.authorization=false
java_option_auth_remote: -Dsas.authorization.remote=false
java_option_eventing: -Dsas.event.enabled=false

3.Set Core Threads to Number of CPU Cores
EnvironmentManager -> Configuration -> Micro Analytic Score service -> sas.microanalyticservice.system -> core -> numthreads
numthreads: <PUT_YOUR_CORE_COUNT_HERE>

4.Allocate Enough JVM Memory
EnvironmentManager -> Configuration -> Micro Analytic Score service -> jvm
java_option_xmn: -Xmn8g
java_option_xms: -Xms16g
java_option_xmx: -Xmx16g
java_option_xss: -Xss512k

5.Linux TCP
echo 30 > /proc/sys/net/ipv4/tcp_fin_timeout
echo 1  > /proc/sys/net/ipv4/tcp_tw_reuse
echo 1  > /proc/sys/net/ipv4/tcp_tw_recycle

6.HTTPD
/etc/httpd/conf.modules.d/00-mpm.conf
#LoadModule mpm_prefork_module modules/mod_mpm_prefork.so 
LoadModule mpm_worker_module modules/mod_mpm_worker.so 
#LoadModule mpm_event_module modules/mod_mpm_event.so

<IfModule mpm_worker_module>
  ServerLimit          32
  StartServers         10
  MaxRequestWorkers    1024
  MinSpareThreads      25
  MaxSpareThreads      75
  ThreadsPerChild      32
  MaxConnectionsPerChild  0
</IfModule>
```
