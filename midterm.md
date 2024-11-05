# 期中練習

## 題目：
You need to create two vpc networks, i.e. myvpc1 and myvpc2. In myvpc1 (any zone), create two VMs with http server. In myvpc2, create DB (vm or cloud sql). make http servers connect to DB. Also add one load balancer. If a customer connect to LB, LB will dispatch the traffic to the backend http server.<br>
![](src/midterm.jpg)

## 作法：
1. 建立兩個VPC network：
    1. Name: `myvpc1`
        - Subnet creation mode: `custom`
            - Subnet name: `asia-east1`
            - Subnet IPv4 range: `10.10.0.0/24`
        - Firewall rules:
            - myvpc1-allow-icmp
            - myvpc1-allow-ssh
    2. Name: `myvpc2`
        - Subnet creation mode: `custom`
            - Subnet name: `asia-east2`
            - Subnet IPv4 range: `10.20.0.0/24`
        - Firewall rules:
            - myvpc2-allow-icmp
            - myvpc2-allow-ssh
            - 額外新增規則：myvpc2-allow-3306
                - allow `TCP:3306`
2. 建立VPC peering：
    1. Name: `myvpc1-myvpc2`
        - Your VPC network: `myvpc1`
        - VPC network name: `myvpc2`
    2. Name: `myvpc2-myvpc1`
        - Your VPC network: `myvpc2`
        - VPC network name: `myvpc1`
3. 建立3台虛擬機：
    1. Name: `www1`
        - Region: `asia-east1 (Taiwan)`
        - Machine configuration: `N1`
        - OS: `Ubuntu 20.04 LTS`
        - Firewall: `Allow HTTP traffic`
        - Advanced options -> Network interfaces: `myvpc1`
        - Advanced options -> Management: Automation
            ```bash
            #!/bin/bash
            apt update
            apt -y install apache2
            cat <<EOF > /var/www/html/index.html
            <html><body><p>Linux startup script added directly. $(hostname -I)</p></body></html>
            ```
    2. Name: `www2`
        - Region: `asia-east1 (Taiwan)`
        - Machine configuration: `N1`
        - OS: `Ubuntu 20.04 LTS`
        - Firewall: `Allow HTTP traffic`
        - Advanced options -> Network interfaces: `myvpc1`
        - Advanced options -> Management: Automation
            ```bash
            #!/bin/bash
            apt update
            apt -y install apache2
            cat <<EOF > /var/www/html/index.html
            <html><body><p>Linux startup script added directly. $(hostname -I)</p></body></html>
            ```
    3. Name: `mydb`
        - Region: `asia-east1 (Taiwan)`
        - Machine configuration: `N1`
        - OS: `Ubuntu 20.04 LTS`
        - Advanced options -> Network interfaces: `myvpc2`
4. 在`mydb`安裝`mariadb`
    1. 安裝：<br>
        ```bash
        sudo apt install mariadb-server
        ```
    2. 啟動：<br>
        ```bash
        sudo systemctl start mariadb
        ```
    3. 設定：<br>
        ```bash
        sudo mysql_secure_installation
        ```
        ```
        Enter current password for root (Enter for none): Enter
        Change the root password? [Y/n]: y
        New password: 123456
        Re-enter new password: 123456
        Remove anonymous users? [Y/n]: y
        Disallow root login remotly? [Y/n]: n
        Remove test database and access to it? [Y/n]: y
        Reload privilege tables now? [Y/n]: y
        ```
    4. 測試登入mysql並新增資料庫：<br>
        ```bash
        sudo mysql -uroot -p
        ```
        設定資料庫權限<br>
        ```sql
        GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
        FLUSH PRIVILEGES;
        ```
