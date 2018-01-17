# speltdownpatcher

This is just a basic ansible recipie for patch servers in an automatized way against spectre and meltdown.
For test if the servers are vulnerable it implements the follow script:

https://github.com/speed47/spectre-meltdown-checker

(thanks speed47 for wrote it)

The recipie works in this way:

- First it downloads the script in each server
- Execute the script in all of the servers, set facts if the server is vulnerable to the three variants
- In case the server is vulnerable to variant 1 and three, patch the servers (there are not patchs yet for the varaint 2!!)
- Once doing that, evalute if the servers is going to reboot
- Write a report if the server was rebooted or not
- Run once again the check of vulnerabilities and write a report if the system till is vulerabble

## How to use it

There are 2 groups in the hosts file, one is named withReboot and the other is withoutReboot, the aim of this is controll whichs servers you want to reboot, or you just could pass to the role "mitigateBugs" the var "rebootServers" either with the value "yes" or "y".

The reports will be save in the local host where ansible is running, the are two defaults variables for define the path and the name of the report:

pathReportVulnerabilites and pathRebootReport, the first defines where the log about if the servers are vulnerable will be saved, and the second where the log about if the server was rebooted.

The default value for pathReportVulnerabilites is /tmp/serverStateVulnerabilities and for pathRebootReport it is /tmp/serversRebootedState, so in case you want another path, just pass it to the correspond role.

After that you just run the the playbook with:

ansible-playbook -i hosts pb.yaml

If you only want to check if the systems are vulnerable and generate a report of it, run the next:

ansible-playbook -i hosts pb.yaml --tags=checkVuln --extra-vars "statVulnReport=1"

Or just reboot the servers

ansible-playbook -i hosts pb.yaml --tags=rebootServers

And wait until it finish
