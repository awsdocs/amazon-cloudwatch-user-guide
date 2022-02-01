# Amazon Elastic Container Service \(Amazon ECS\)<a name="appinsights-metrics-ecs"></a>

CloudWatch Application Insights supports the following metrics:

**Topics**
+ [CloudWatch built\-in metrics](#appinsights-metrics-ecs-built-in-metrics)
+ [Container Insights metrics](#appinsights-metrics-ecs-container-insights-metrics)
+ [Container Insights Prometheus metrics](#appinsights-metrics-ecs-container-insights-prometheus)

## CloudWatch built\-in metrics<a name="appinsights-metrics-ecs-built-in-metrics"></a>

CPUReservation

CPUUtilization

MemoryReservation

MemoryUtilization

GPUReservation

## Container Insights metrics<a name="appinsights-metrics-ecs-container-insights-metrics"></a>

ContainerInstanceCount

CpuUtilized

CpuReserved

DeploymentCount

DesiredTaskCount

MemoryUtilized

MemoryReserved

NetworkRxBytes

NetworkTxBytes

PendingTaskCount

RunningTaskCount

ServiceCount

StorageReadBytes

StorageWriteBytes

TaskCount

TaskSetCount

instance\_cpu\_limit

instance\_cpu\_reserved\_capacity

instance\_cpu\_usage\_total

instance\_cpu\_utilization

instance\_filesystem\_utilization

instance\_memory\_limit

instance\_memory\_reserved\_capacity

instance\_memory\_utilization

instance\_memory\_working\_set

instance\_network\_total\_bytes

instance\_number\_of\_running\_tasks

## Container Insights Prometheus metrics<a name="appinsights-metrics-ecs-container-insights-prometheus"></a>

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