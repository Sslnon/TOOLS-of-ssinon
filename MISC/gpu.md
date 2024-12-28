> 这里是用于监控简单实验室集群的脚本
> 
> 使用前请配置集群免密和gpustat

```shell
#!/bin/bash

servers=("gpuxx" "gpuxx")
output_dir="/path/to/your/tmp/"
interval=10 # 设置你需要的间隔时间 

mkdir -p "$output_dir"

first_iteration=true

while true; do
  if [ "$first_iteration" = true ]; then
    first_iteration=false
  else
    sleep "$interval"
  fi

  echo -e "\e[1;34m========== GPU - $(date) ==========\e[0m"
  
  for server in "${servers[@]}"; do
    (
      ssh $server "gpustat --color" > "$output_dir/${server}.log" 2>&1 || echo -e "\e[1;31mfail: $server\e[0m" > "$output_dir/${server}.log"
    ) &
  done
  wait
  
  clear
  for server in "${servers[@]}"; do
    echo -e "\n\e[1;35m$server GPU state:\e[0m"
    cat "$output_dir/${server}.log"
  done
  
done
```
