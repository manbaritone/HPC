```
# tar -xvf csds-2020.0-linux-x64.tar 
# ./csds-linux-x64.run 

# cd /apps/CCDC/CSD_2020/csd 
# ../bin/ccdc_activator -k XXXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX -g -f ./master_license.dat -A
```

### Generate license offline
https://www.ccdc.cam.ac.uk/support-and-resources/support/case/?caseid=8f5d786c-682d-ea11-978f-005056977c87
```
# ssh compute-0-0
# cd /apps/CCDC/CSD_2020/csd 
# ../bin/ccdc_activator -k XXXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX -g -f ./c0_license.dat -A
# exit
```

### Online Registration
```
# cd /apps/CCDC/CSD_2019/csd
# ../bin/ccdc_activator -k XXXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX-XXXXXX -a -o -f ./c0_license.dat -A
```

### For using with SGE
```
#!/bin/sh
#$ -cwd
#$ -j y

#$ -S /bin/sh

export MPI=/opt/openmpi/bin/
export LD_LIBRARY_PATH=/opt/openmpi/lib:/opt/openmpi/lib/openmpi:$LD_LIBRARY_PATH

# Define path of Gold directory.
export GOLD_DIR=“/state/partition4/home_other/nobackup/kiattawee/CCDC/GoldSuite_2018/"

# Define path of Gold license file.
export CCDC_LICENSE_FILE=/state/partition4/home_other/nobackup/kiattawee/CCDC/CSD_2018/csd/csd_licence.dat

# Add Gold lib.
export LD_LIBRARY_PATH=${GOLD_DIR}/c_linux-64/lib:${LD_LIBRARY_PATH}


###############################################

# Run Gold program.
${GOLD_DIR}/bin/gold_auto gold.conf

###############################################
```

### Licence server setup
```
chmod a+x ccdc_licence_server-linux-x64-installer.run
./ccdc_licence_server-linux-x64-installer.run
```
make service script
```
vi /etc/systemd/system/ccdcfloatserver.service

[Unit]
Description=ccdc-floating-server

After=network.target local-fs.target multi-user.target
Requires=network.target local-fs.target multi-user.target

[Service]
Type=simple
Restart=always
RestartSec=3
User=root
ExecStart=/apps/CCDC/CCDCLicServer/CCDCFloatServer -s -productfile=/apps/CCDC/CCDCLicServer/ccdc.dat -config=/apps/CCDC/CCDCLicServer/ccdc.config -silent

[Install]
WantedBy=multi-user.target
```
Setup port by ccdc.config
```
# Port LexFloatServer should bind to
port=8880


# run to make ccdc.dat
./CCDCFloatServer -a -licensekey=123456-123456-123456-123456-123456-123456 -config=./ccdc.config -productfile=./ccdc.dat

# start the service
systemctl start ccdcfloatserver

```
