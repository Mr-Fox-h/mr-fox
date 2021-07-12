---
layout: post
title: "Metasploit"
permalink: /posts/metasploit/
---
![Metasploit](https://th.bing.com/th/id/R.12958ada2ecca5e6e8458cf11211b429?rik=6h87%2ffzMtn1EEA&pid=ImgRaw)

---

سلامی دوباره

`Metasploit` یک از ابزارهای قدرتمندی است که
یک هکر می تواند از آن استفاده کند.

`Metasploit` یک ابزار قدرتمند و متن باز هستش که
هکرها از آن استفاده می کنند برای تست نفوذ و هک که این ابزار از جنبه علمی، به شدت دارای
اهمیت است و خیلی از محققان `IT` از این ابزار بهره می گیرند.


بیشترین کاربرد این ابزار، ایجاد یک راه ارتباطی با سیستم های هدف است.
یعنی هکرها با این ابزار تلاش می کنند تا با یک روشی بتوانند به سیستم های هدف خود حمله کنند و بتوانند سیستم های هدف را به کنترول خود در آورند و این یکی از کار هایی است که این ابزار می تواند انجام دهد.

### توانایی های Metasploit

این ابزار توانایی های بسیار زیادی دارد و به یکی از آن ها در همین بالای متن به آن اشاره کرده ایم، و اما بقیه توانایی های این ابزار:

1_با `Metasploit` توانایی شناسایی سیستم های هدف را دارید.`(اطلاعات جمع آوری کنید و مشکلات آن ها را برسی کنید.)`

2_با `Metasploit` توانایی حمله به سیستم های هدف امکان پذیر است.`(این کار رو نکنید)`

3_با `Metasploit` توانایی ساخت `Payload` های متفاوت رو دارید.`(برای هر سیستم عاملی و هر نوع کاری)`

4_با `Metasploit` توانایی کد گذاری برای `Payload` ها رو دارید تا بتوان `Anti-Virus` ها رو دور زد و ...

به امید خدا قرار که یک سری آموزش هم از `Metasploit` برای شما عزیزان قرار بدهم تا شما دوستان بتوانید  با این ابزار بیشتر آشنایی پیدا کنید و بتوانید از آن در راه درست استفاده کنید، فقط قبل از اون پیشنهاد می کنم که یک ماشین مجازی و هم [Metasploitable][Metasploitable] داشته باشید تا شما دوستان بتوانید تمام کارها رو در همان [Metasploitable][Metasploitable] انجام دهید.

طریقه نصب `Metasploit`:

### قدم اول
پایگاه داده `PostgreSQL` نصب شده است اما در `Kali Linux` آغاز نشده است. با استفاده از دستور زیر سرویس را شروع کنید.

```
$ sudo systemctl enable --now postgresql
```

تأیید کنید که سرویس شروع شده است و تنظیم می شود که از طریق بوت اجرا شود.

```
$ systemctl status postgresql@12-main.service
● postgresql@12-main.service - PostgreSQL Cluster 12-main
     Loaded: loaded (/lib/systemd/system/postgresql@.service; enabled-runtime; vendor preset: disabled)
    Drop-In: /usr/lib/systemd/system/postgresql@.service.d
             └─kali_postgresql.conf
     Active: active (running) since Sun 2020-02-23 07:24:55 EST; 43s ago
    Process: 16154 ExecStartPre=/usr/share/kali-defaults/postgresql_reduce_shared_buffers 12/main (code=exited, status=0/SUCCESS)
    Process: 16156 ExecStart=/usr/bin/pg_ctlcluster --skip-systemctl-redirect 12-main start (code=exited, status=0/SUCCESS)
   Main PID: 16184 (postgres)
      Tasks: 7 (limit: 2318)
     Memory: 28.6M
     CGroup: /system.slice/system-postgresql.slice/postgresql@12-main.service
             ├─16184 /usr/lib/postgresql/12/bin/postgres -D /var/lib/postgresql/12/main -c config_file=/etc/postgresql/12/main/postgresql.conf
             ├─16186 postgres: 12/main: checkpointer
             ├─16187 postgres: 12/main: background writer
             ├─16188 postgres: 12/main: walwriter
             ├─16189 postgres: 12/main: autovacuum launcher
             ├─16190 postgres: 12/main: stats collector
             └─16191 postgres: 12/main: logical replication launcher

$ systemctl is-enabled postgresql
enabled
```

با بررسی خروجی ss -ant و اطمینان از گوش دادن پورت 5432 می توانید بررسی کنید که `PostgreSQL` در حال اجرا است.
```
$ sudo  ss -ant | grep 5432
LISTEN  0       244            127.0.0.1:5432           0.0.0.0:*
LISTEN  0       244                [::1]:5432              [::]:* 
```

### قدم دوم

هنگامی که سرور پایگاه داده `PostgreSQL` در حال اجرا است ، شروع به تنظیم اولیه پایگاه داده `Metasploit PostgreSQL` می کنید.

```
$ sudo msfdb init
[i] Database already started
[+] Creating database user 'msf'
[+] Creating databases 'msf'
[+] Creating databases 'msf_test'
[+] Creating configuration file '/usr/share/metasploit-framework/config/database.yml'
[+] Creating initial database schema
```

با این کار پایگاه داده msf ایجاد و مقداردهی اولیه می شود.

### قدم سوم

اکنون سرویس `PostgreSQL` در حال اجرا و راه اندازی است و پایگاه داده را مقدماتی آغاز کرده است. آخرین مرحله مورد نیاز راه اندازی `msfconsole` و تأیید اتصال پایگاه داده با دستور `db_status` است:

```
$ sudo msfconsole
                                                  
                                              `:oDFo:`                            
                                           ./ymM0dayMmy/.                          
                                        -+dHJ5aGFyZGVyIQ==+-                    
                                    `:sm⏣~~Destroy.No.Data~~s:`                
                                 -+h2~~Maintain.No.Persistence~~h+-              
                             `:odNo2~~Above.All.Else.Do.No.Harm~~Ndo:`          
                          ./etc/shadow.0days-Data'%20OR%201=1--.No.0MN8'/.      
                       -++SecKCoin++e.AMd`       `.-://///+hbove.913.ElsMNh+-    
                      -~/.ssh/id_rsa.Des-                  `htN01UserWroteMe!-  
                      :dopeAW.No<nano>o                     :is:TЯiKC.sudo-.A:  
                      :we're.all.alike'`                     The.PFYroy.No.D7:  
                      :PLACEDRINKHERE!:                      yxp_cmdshell.Ab0:    
                      :msf>exploit -j.                       :Ns.BOB&ALICEes7:    
                      :---srwxrwx:-.`                        `MS146.52.No.Per:    
                      :<script>.Ac816/                        sENbove3101.404:    
                      :NT_AUTHORITY.Do                        `T:/shSYSTEM-.N:    
                      :09.14.2011.raid                       /STFU|wall.No.Pr:    
                      :hevnsntSurb025N.                      dNVRGOING2GIVUUP:    
                      :#OUTHOUSE-  -s:                       /corykennedyData:    
                      :$nmap -oS                              SSo.6178306Ence:    
                      :Awsm.da:                            /shMTl#beats3o.No.:    
                      :Ring0:                             `dDestRoyREXKC3ta/M:    
                      :23d:                               sSETEC.ASTRONOMYist:    
                       /-                        /yo-    .ence.N:(){ :|: & };:    
                                                 `:Shall.We.Play.A.Game?tron/    
                                                 ```-ooy.if1ghtf0r+ehUser5`    
                                               ..th3.H1V3.U2VjRFNN.jMh+.`          
                                              `MjM~~WE.ARE.se~~MMjMs              
                                               +~KANSAS.CITY's~-`                  
                                                J~HAKCERS~./.`                    
                                                .esc:wq!:`                        
                                                 +++ATH`                            
                                                  `


       =[ metasploit v5.0.76-dev                          ]
+ -- --=[ 1971 exploits - 1088 auxiliary - 339 post       ]
+ -- --=[ 558 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]
msf5 > 
```

اتصال پایگاه داده را تأیید کنید.

```
msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
msf5 > 
```

از آنجا که `Metasploit Framework` بخشی از سیستم عامل `Kali Linux` است ، از طریق بسته `apt` به روز می شود.

```
sudo apt update
sudo apt install metasploit-framework
```

---

با تشکر از شما

[Metasploitable]: http://mr-fox.blog.ir/1399/10/27/Metasploitable
