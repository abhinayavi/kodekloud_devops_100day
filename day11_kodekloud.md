# Day 11 of 100 DAY KodeKoud Task - Tomcat Deployment

## Problem Statement
The Nautilus application development team recently finished the beta version of one of their Java-based applications, which they are planning to deploy on one of the app servers in Stratos DC. After an internal team meeting, they have decided to use the tomcat application server. Based on the requirements mentioned below complete the task:

- **a.** Install tomcat server on App Server 3.  
- **b.** Configure it to run on port 8084.  
- **c.** There is a `ROOT.war` file on Jump host at location `/tmp`. Deploy it on this tomcat server and make sure the webpage works directly on base URL i.e `curl http://stapp03:8084`  

---

## Solution

### Step 1: Connect to stapp03 Server
```bash
ssh banner@stapp03
Password: BigGr33n
```

### Step 2: Install Tomcat and Required Packages
```bash
sudo yum install -y tomcat tomcat-webapps
```

### Step 3: Configure Tomcat to Run on Port 8084
Edit the Tomcat server configuration file:
```bash
sudo vi /etc/tomcat/server.xml
```

Change the connector port from **8080** to **8084**:

```xml
<Connector port="8084" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" />
```

### Step 4: Start and Enable Tomcat Service
```bash
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

### Step 5: Transfer ROOT.war from Jump Host to stapp03
From the jump host, execute:
```bash
scp /tmp/ROOT.war banner@stapp03:/tmp/
```

### Step 6: Deploy ROOT.war on Tomcat
On **stapp03**, execute:
```bash
sudo cp /tmp/ROOT.war /var/lib/tomcat/webapps/
sudo chown tomcat:tomcat /var/lib/tomcat/webapps/ROOT.war
```

### Step 7: Restart Tomcat Service
```bash
sudo systemctl restart tomcat
```

### Step 8: Verify the Deployment
```bash
curl http://localhost:8084
```

**Expected Output:**
```html
<!DOCTYPE html>
<html>
    <head>
        <title>SampleWebApp</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
    </head>
    <body>
        <h2>Welcome to xFusionCorp Industries!</h2>
        <br>
    </body>
</html>
```

---

## Final Verification from Jump Host
```bash
curl http://stapp03:8084
```
