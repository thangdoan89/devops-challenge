# Troubleshoot NGINX Load Balancer as a single Service on VM

## Possible scenarios
1. High traffic load in rush hours
2. DDoS
3. Wrong configurations led to high resource usage

## Possible troubleshoot steps
### Step 1: Examine External Factors (Traffic or Load), Check Monitoring Service to get logs or metrics
- cmd for getting logs from NGINX:
    - `tail -f /var/log/nginx/access.log` 
    - `tail -f /var/log/nginx/error.log` 
- Target: Detect incoming traffic patterns, traffic load and filter out possibility of DDoS
- Recovery steps:
    - Limit load speed and response in `nginx.conf`: `limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/s;`
    - Blacklist all IPs that cause DDoS or from unusual traffics 
    - Add CDN to load balancing more properly

### Step 2: Check System Resource Usage: ram, cpu, disk, swap,...
- cmd: 
    - `free -h`, `htop`
    - `df -h`: Check disk usage
    - `du -sh /var/log/nginx/*`: Check nginx logs files
    - `swapon -s`: Check swap status or use `htop`
- Target: Detect whether NGINX or any processes are consuming resources 
- Steps:
    - Stop processes that are not from OS and NGINX
    - Check the status of memory to detect OOM issue or disk usage raised

### Step 3: Check NGINX Configuration and Traffic:
- Check:
    + NGINX memory usage: `ps aux | grep nginx`
    + NGINX configurations: worker_process, buffers, caching
- Target: If the configurations are not being limited correctly it could cause memory exhaustion in high traffic scenarios
- Các bước khắc phục:
    - Reduce worker_process, buffers, cache size -> restart service
    - Re-test load balancing and checking on resource usage
    
### Step 4: Check for Memory Leaks
- cmd: `pmap` or `smem` to check memory usage by time
- Target: detect memory leaks cause (whether NGINX or other service run along with Load Balancer)
- Recovery steps:
    - Detect and update or remove modules that causes error and leak
    - If not from NGINX, check other leaks that caused by other services
    - Restart NGINX service to temporary return to working state and investigate later on
