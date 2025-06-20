# Monitoring_with_Prometheus_-_Grafana

# Questions

### Question 1: Container Orchestration Analysis 
Examine the Docker Compose configuration used in this lab. Explain why the Node Exporter 
container requires mounting host directories (/proc, /sys, /) and what would happen if these 
mounts were removed or configured incorrectly. How does this design decision reflect the 
principle of containerized monitoring? 

# Answer: 

### Question 2: Network Security Implications 
The lab creates a custom Docker network named "prometheus-grafana". Analyze the security 
implications of this setup versus using the default Docker network. What potential 
vulnerabilities could arise from the current port exposure configuration (9090, 9100, 3000), and 
how would you modify the setup for a production environment? 

### Question 3: Data Persistence Strategy 
Compare the volume mounting strategies used for Prometheus data versus Grafana data in the 
Docker Compose file. Explain why different approaches might be needed for different 
components and what would happen to your dashboards and historical metrics if you removed 
these volume configurations.
 
