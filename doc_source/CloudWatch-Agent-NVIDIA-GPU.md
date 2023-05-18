# Collect NVIDIA GPU metrics<a name="CloudWatch-Agent-NVIDIA-GPU"></a>

You can use the CloudWatch agent to collect NVIDIA GPU metrics from Linux servers\. To set this up, add a `nvidia_gpu` section inside the `metrics_collected` section of the CloudWatch agent configuration file\. For more information, see [Linux section](CloudWatch-Agent-Configuration-File-Details.md#CloudWatch-Agent-Linux-section)\.

The following metrics can be collected\. All of these metrics are collected with no CloudWatch `Unit`, but you can specify a unit for each metric by adding a parameter to the CloudWatch agent configuration file\. For more information, see [Linux section](CloudWatch-Agent-Configuration-File-Details.md#CloudWatch-Agent-Linux-section)\.


| Metric | Metric name in CloudWatch | Description | 
| --- | --- | --- | 
|  `utilization_gpu` |  `nvidia_smi_utilization_gpu` |  The percentage of time over the past sample period during which one or more kernals on the GPU was running\.  | 
|  `temperature_gpu` |  `nvidia_smi_temperature_gpu` |  The core GPU temperature in degrees Celsius\.  | 
|  `power_draw` |  `nvidia_smi_power_draw` |  The last measured power draw for the entire board, in watts\.  | 
|  `utilization_memory` |  `nvidia_smi_utilization_memory` |  The percentage of time over the past sample period during which global \(device\) memory was being read or written\.  | 
|  `fan_speed` |  `nvidia_smi_fan_speed` |  The percentage of maximum fan speed that the device's fan is currently intended to run at\.  | 
|  `memory_total` |  `nvidia_smi_memory_total` |  Reported total memory, in MB\.  | 
|  `memory_used` |  `nvidia_smi_memory_used` |  Memory used, in MB\.  | 
|  `memory_free` |  `nvidia_smi_memory_free` |  Memory free, in MB\.  | 
|  `pcie_link_gen_current` |  `nvidia_smi_pcie_link_gen_current` |  The current link generation\.  | 
|  `pcie_link_width_current` |  `nvidia_smi_pcie_link_width_current` |  The current link width\.  | 
|  `encoder_stats_session_count` |  `nvidia_smi_encoder_stats_session_count` |  Current number of encoder sessions\.  | 
|  `encoder_stats_average_fps` |  `nvidia_smi_encoder_stats_average_fps` |  The moving average of the encode frames per second\.  | 
|  `encoder_stats_average_latency` |  `nvidia_smi_encoder_stats_average_latency` |  The moving average of the encode latency in microseconds\.  | 
|  `clocks_current_graphics` |  `nvidia_smi_clocks_current_graphics` |  The current frequency of the graphics \(shader\) clock\.  | 
|  `clocks_current_sm` |  `nvidia_smi_clocks_current_sm` |  The current frequency of the Streaming Multiprocessor \(SM\) clock\.  | 
|  `clocks_current_memory` |  `nvidia_smi_clocks_current_memory` |  The current frequency of the memory clock\.  | 
|  `clocks_current_video` |  `nvidia_smi_clocks_current_video` |  The current frequency of the video \(encoder plus decoder\) clocks\.  | 

All of these metrics are collected with the following dimensions:


| Dimension | Description | 
| --- | --- | 
|  `index` |  A unique identifier for the GPU on this server\. Represents the NVIDIA Management Library \(NVML\) index of the device\.  | 
|  `name` |  The type of GPU\. For example, `NVIDIA Tesla A100`  | 
|  `host` |  The server host name\.  | 