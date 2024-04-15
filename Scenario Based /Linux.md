# Linux based Scenarios Questions

#### 1) Can you describe a scenario where you need to troubleshoot a performance issue on a Linux server?

**Ans**: Sure. Let's say there is a web application running on a Linux server, and users are reporting slow response times. The first step would be to check the system resource utilization using commands like `top`, `htop`, or `ps`. This would give an overview of CPU, memory, and disk usage.

#### 2) let's say the CPU usage is high. How would you identify the processes consuming the most CPU?

**ANS**: I would use the 'top' command and sort processes by CPU usage. Alternatively, I could use `ps` with options like `aux --sort=-%cpu' to list processes by CPU consumption. Once identified, I would investigate those processes further to understand why they are consuming so much CPU.

#### 3) Suppose the CPU usage seems normal but the memory usage is high. How would you troubleshoot this issue?

**ANS**: Check Overall Memory Usage:
Use **free -h** to see how much memory is being used and available.

Identify Memory-Hungry Processes:
Run **top** or **htop** to find processes using the most memory. Press **Shift + M ** in **top** to sort by memory usage.

Inspect Process Memory Usage:
Use **ps aux --sort=-%mem** to see detailed memory usage by processes.

Look for Memory Leaks:
Examine potential memory leaks in processes using **pmap -x <PID>.**

Check System Logs:
Review logs **(/var/log/messages, /var/log/syslog)** for memory-related issues.

Monitor System Statistics:
Use **vmstat 1** to monitor memory usage over time and watch for unusual spikes.

Investigate Kernel Memory:
Check kernel memory usage with tools like **slabtop** or **cat /proc/meminfo**.

Check for Out-of-Memory Events:
Look for OOM events with **dmesg | grep -i oom** to see if the kernel is killing processes due to memory shortages.

Review System Configuration:
Ensure your system's memory settings (**sysctl** parameters) are appropriate.

Optimize Applications:
If specific applications are causing high memory usage, review and optimize their configurations.

#### 4) Can you explain how would you automate the deployment process of a web application on multiple linux servers?

**ANS**: Absolutely. I would use a configuration management tool like Ansible, Puppet, or Chef to automate the deployment process. I'd write playbooks or manifests to define the desired state of each server, including installing dependencies, configuring web servers, deploying the application code, and restarting services if needed. With these tools, I can ensure consistency across all servers and easily scale the deployment as needed.


#### 5) One last question: How would you ensure the security of the Linux servers in your environment?

**ANS**: Security is paramount in any environment. To secure Linux servers, I would implement best practices such as regularly applying security patches, using strong passwords and SSH key-based authentication, configuring firewalls (like iptables or firewalld) to restrict access, enabling SELinux.

## Some troubleshooting issues

1) Email delivery failure

**Issue**: Email messages not being delivered

**Solution**: 1.Start by checking the mail logs on your Linux system.
             **tail -f /var/log/mail.log**

              2.Ensure that your Linux system can resolve DNS queries correctly. Use tools like ping or nslookup to verify DNS resolution for the recipient's mail server.
                 **nslookup recipientdomain.com**
             


