# For deplyment via Boot-script(User-data) in AWS

## Guide to deploy via User-Data for sqli lab:
* [SQLI](sqli-penTestBootScript.md)

## Deploy via bash script
1. SSH in your instance
2. Follow the code below
```
sudo bash
git clone xxx
cd path/to/sqli-penTestLab.sh
chmod +x sqli-penTestLab.sh
./sqli-penTestLab.sh
```
## Bash Script
* [SQLI](sqli-penTestLab.sh)

## Write-ups for SQLi
* [SQLI](sqli-writeup.md)

# References

https://alestic.com/2010/12/ec2-user-data-output/

https://www.hackingarticles.in/web-application-pentest-lab-setup-on-aws/