#!/bin/bash
read -p " enter the num " num
if [ "$num" -gt 0 ]; then
        echo " the $num is positive "
elif [ "$num" -eq 0 ]; then
        echo " the $num is zero  "
else
        echo " the $num is negative "
fi

 output 
 enter the num 2
 the 2 is positive 
  2)
  #!/bin/bash
 read -p " enter the file name :" file
 if [  -f $file  ] ;then
         echo "  the file exits "
         exit 1
 else
         echo " file not exits "
 fi

output
enter the file name :hello.sh
  the file exits 
  
  #!/bin/bash
 read -p "  enter your name plz " name
 echo " welcome $namw "
 read -p " enter your favourite tool " tool
 echo " hello $name , your favourite tool is $tool "
~                                                        output enter your name plz jana
 welcome  
 enter your favourite tool docker
 hello jana , your favourite tool is docker 


 #!/bin/bash
 read -p " enetr the name of the service " service
   read -p  "Do you want to check the status? (y/n) :" check
   if [ $check="y" ]; then
           echo " *********** checking the service ******** "
          sudo  systemctl status $service
          else
                  echo " **** skipped ****"
   fi

  Do you want to check the status? (y/n) :y
 *********** checking the service ******** 
[sudo] password for jana: 
Sorry, try again.
[sudo] password for jana: 
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Sat 2026-02-28 20:20:05 IST; 1h 49min ago
       Docs: man:nginx(8)
    Process: 1850 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 1857 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 1862 (nginx)
      Tasks: 13 (limit: 18605)
     Memory: 10.3M (peak: 11.8M)
        CPU: 65ms
     CGroup: /system.slice/nginx.service
             ├─1862 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             ├─1865 "nginx: worker process"
             ├─1866 "nginx: worker process"
             ├─1867 "nginx: worker process"
             ├─1868 "nginx: worker process"
             ├─1869 "nginx: worker process"
             ├─1872 "nginx: worker process"
             ├─1873 "nginx: worker process"
             ├─1874 "nginx: worker process"
             ├─1875 "nginx: worker process"
             ├─1876 "nginx: worker process"
             ├─1877 "nginx: worker process"
             └─1878 "nginx: worker process"

Feb 28 20:20:05 jana-Nitro-AN515-58 systemd[1]: Starting nginx.service - A high performance web server and a reverse proxy server...
Feb 28 20:20:05 jana-Nitro-AN515-58 systemd[1]: Started nginx.service - A high performance web server and a reverse proxy server.
 output 


 #!/bin/bash
 name='jana'
 role='devops engineer'
  echo " hello im $name and im a $role "
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                                                                                                                                                                             
~                                               
