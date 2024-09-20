# Prepare S3 Bucket and Clone Files

We will use AWS Cloud9 to run shell commands, edit and run Python scripts for the labs. Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It combines the rich code editing features of an IDE such as code completion, hinting, and step-through debugging, with access to a full Linux server for running and storing code.

### Prepare Cloud9 Variables, Local Directories, Files & Workshop Configurations
**1** Go to the AWS Cloud9 console in your environment and you should see a Cloud9 with name glueworkshop. Click Open IDE button to enter Cloud9 IDE environment.

**2** In Cloud9, click on the menu bar **Window** and then **New Terminal** . This will create a new tab and load a command-line terminal. You will use this terminal window throughout the lab to execute the AWS CLI commands and scripts.

**3** Copy below commands (always use the tiny copy icon on the top-right-corner of the code block!!!) and paste it in your **Cloud9 Command Line Terminal:**

```
cd ~/environment/

echo "==================> DOWNLOADING & EXECUTING THE ONE-STEP-SETUP SCRIPT <====================
$(curl -s 'https://static.us-east-1.prod.workshops.aws/a7a18ca2-c158-4f3d-900a-efba04e04528/static/download/howtostart/awseevnt/s3-and-local-file/one-step-setup.sh?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9hN2ExOGNhMi1jMTU4LTRmM2QtOTAwYS1lZmJhMDRlMDQ1MjgvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTcyNzQ1NTgzMX19fV19&Signature=Ta26U2Hwz79U2nk3gD~qEKKjfT3VogbBkIQmKCLVIgztuyVmYneKEL2oQFj4K8wRfv4PtNJ~5ptLyNNHJMxc084hNPKfd5FbDWzz4aMU-5qCasTO0chwGzLEj54MsHFwwTQu0hYkHHulJahdUVe9IgzlQ-9pnptQtwfNvqnIf1zb88hLciuCxZHcnQYG-17YkKqC7TWch~KofDUP2Wu5TpY~2wHfMWhMcGm8Q8OKErEb7ruAXda5R5V78emxScfqdBTs7yPmEsDJLg7y~Z2QQes-U1pe~ksqC4v9EvYtPoYq1zEDvYMSFu9hTjX0FtbIDCSceO28iJ2KwY6X-qYnXw__' --output ~/environment/glue-workshop/library/one-step-setup.sh --create-dirs)
==========================================================================================="

. ./glue-workshop/library/one-step-setup.sh  'https://static.us-east-1.prod.workshops.aws/a7a18ca2-c158-4f3d-900a-efba04e04528/static/0/?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy9hN2ExOGNhMi1jMTU4LTRmM2QtOTAwYS1lZmJhMDRlMDQ1MjgvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTcyNzQ1NTgzMX19fV19&Signature=Ta26U2Hwz79U2nk3gD~qEKKjfT3VogbBkIQmKCLVIgztuyVmYneKEL2oQFj4K8wRfv4PtNJ~5ptLyNNHJMxc084hNPKfd5FbDWzz4aMU-5qCasTO0chwGzLEj54MsHFwwTQu0hYkHHulJahdUVe9IgzlQ-9pnptQtwfNvqnIf1zb88hLciuCxZHcnQYG-17YkKqC7TWch~KofDUP2Wu5TpY~2wHfMWhMcGm8Q8OKErEb7ruAXda5R5V78emxScfqdBTs7yPmEsDJLg7y~Z2QQes-U1pe~ksqC4v9EvYtPoYq1zEDvYMSFu9hTjX0FtbIDCSceO28iJ2KwY6X-qYnXw__'
```
