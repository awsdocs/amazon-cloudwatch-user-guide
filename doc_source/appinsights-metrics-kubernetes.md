# Kubernetes on AWS<a name="appinsights-metrics-kubernetes"></a>

CloudWatch Application Insights supports the following metrics:

**Topics**
+ [Container Insights metrics](#appinsights-metrics-kubernetes-container-insights-metrics)
+ [Container Insights Prometheus metrics](#appinsights-metrics-kubernetes-container-insights-prometheus)

## Container Insights metrics<a name="appinsights-metrics-kubernetes-container-insights-metrics"></a>

cluster\_failed\_node\_count

cluster\_node\_count

namespace\_number\_of\_running\_pods

node\_cpu\_limit

node\_cpu\_reserved\_capacity

node\_cpu\_usage\_total

node\_cpu\_utilization

node\_filesystem\_utilization

node\_memory\_limit

node\_memory\_reserved\_capacity

node\_memory\_utilization

node\_memory\_working\_set

node\_network\_total\_bytes

node\_number\_of\_running\_containers

node\_number\_of\_running\_pods

pod\_cpu\_reserved\_capacity

pod\_cpu\_utilization

pod\_cpu\_utilization\_over\_pod\_limit

pod\_memory\_reserved\_capacity

pod\_memory\_utilization

pod\_memory\_utilization\_over\_pod\_limit

pod\_network\_rx\_bytes

pod\_network\_tx\_bytes

service\_number\_of\_running\_pods

## Container Insights Prometheus metrics<a name="appinsights-metrics-kubernetes-container-insights-prometheus"></a>

**Java JMX metrics**

java\_lang\_memory\_heapmemoryusage\_used

java\_lang\_memory\_heapmemoryusage\_committed

java\_lang\_operatingsystem\_openfiledescriptorcount

java\_lang\_operatingsystem\_maxfiledescriptorcount

java\_lang\_operatingsystem\_freephysicalmemorysize

java\_lang\_operatingsystem\_freeswapspacesize

java\_lang\_threading\_threadcount

java\_lang\_classloading\_loadedclasscount

java\_lang\_threading\_daemonthreadcount

java\_lang\_garbagecollector\_collectiontime\_copy

java\_lang\_garbagecollector\_collectiontime\_ps\_scavenge

java\_lang\_garbagecollector\_collectiontime\_parnew

java\_lang\_garbagecollector\_collectiontime\_marksweepcompact

java\_lang\_garbagecollector\_collectiontime\_ps\_marksweep

java\_lang\_garbagecollector\_collectiontime\_concurrentmarksweep

java\_lang\_garbagecollector\_collectiontime\_g1\_young\_generation

java\_lang\_garbagecollector\_collectiontime\_g1\_old\_generation

java\_lang\_garbagecollector\_collectiontime\_g1\_mixed\_generation

java\_lang\_operatingsystem\_committedvirtualmemorysize