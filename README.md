# Databricks Asset Bundles (DAB) with Github Actions
![image](https://github.com/user-attachments/assets/d6bd9556-6f4d-4f63-b164-073b43817f13)

Pre-requisites - 
Step 1 :Installation of Databricks CLI ( Iam using Mac)

Step 2: Creating Databricks Workspaces dev-dab-workspace and prod-dab-workspace

![image](https://github.com/user-attachments/assets/61922cda-a24c-4955-8251-b862cbdddfec)

Step 3 : Configure Databricks cli after creating token in databricks like below
![image](https://github.com/user-attachments/assets/38f9711d-5d84-4233-925f-ff09498312a7)

![image](https://github.com/user-attachments/assets/6a3339e0-7635-47b9-b869-5483ed8ca575)

Step 4: Create a bundle project
bijum@MacBook-Pro DBK-CICD % databricks bundle init

Welcome to the default Python template for Databricks Asset Bundles!
Please provide the following details to tailor the template to your preferences.

Unique name for this project [my_project]: dab-project
Validation failed: invalid value for project_name: "dab-project". Name must consist of letters, numbers, and underscores.
Please provide the following details to tailor the template to your preferences.

Unique name for this project [my_project]: dabproject
Include a stub (sample) notebook in 'dabproject/src': yes
Include a stub (sample) Delta Live Tables pipeline in 'dabproject/src': yes
Include a stub (sample) Python package in 'dabproject/src': yes
Workspace to use (auto-detected, edit in 'dabproject/databricks.yml'): https://adb-1602702077418624.4.azuredatabricks.net

✨ Your new project has been created in the 'dabproject' directory!

Please refer to the README.md file for "getting started" instructions.
See also the documentation at https://docs.databricks.com/dev-tools/bundles/index.html.

You will see new folder created with new folders and files

![image](https://github.com/user-attachments/assets/14e1c923-4795-4388-8a51-8697ba4ec561)
databricks.yml: This is the main configuration file used by DAB. It contains the environment specific configs which are used when we deploy the same bundle to different environments
src/notebook.ipynb: Contains the main project code. For this tutorial i’m using a simple print statement



Step 5: Validate and Deploy

 >databricks bundle validate

![image](https://github.com/user-attachments/assets/d558d8cb-6ea7-4f11-a4f2-685d078c53f0)

bijum@MacBook-Pro dabproject % databricks bundle deploy -t dev
Building python_artifact...
Error: build failed python_artifact, error: exit status 1, output: Traceback (most recent call last):
  File "/Volumes/D/DBK-CICD/dabproject/setup.py", line 9, in <module>
    from setuptools import setup, find_packages
ModuleNotFoundError: No module named 'setuptools'

Need to fix above error
To solve it , I created a virtual environment with python from VSCode

Make sure to change folder path to virtual environment

.venvbijum@MacBook-Pro dabproject % 

![image](https://github.com/user-attachments/assets/d14adc3a-88bf-4851-9bc6-53e367beb02c)


Step 6- Navigate to dev workspace

![image](https://github.com/user-attachments/assets/b233eea8-9f4d-4b4e-b3ce-eb0a8b2fb59c)

Step 7 - run job from vs code

![image](https://github.com/user-attachments/assets/5e82e211-e625-43e3-80e1-d03f3867b7b4)

Automatically job will trigger in databricks workspace

![image](https://github.com/user-attachments/assets/8583022e-9fb0-43d0-9db1-784b4253abba)


Step 8: Setting up CICD with GitHub Actions

Create a folder “.github” having two files dev.yaml and prod.yaml, these are basically the workflow files used by GitHub Actions

![image](https://github.com/user-attachments/assets/bea6f998-20f6-46ab-8fe4-fe035f4d484a)

bijum@MacBook-Pro dabproject % git init
Initialized empty Git repository in /Volumes/D/DBK-CICD/dabproject/.git/
bijum@MacBook-Pro dabproject % 

bijum@MacBook-Pro dabproject % git add .

bijum@MacBook-Pro dabproject % git commit -m "Initial commit"

![image](https://github.com/user-attachments/assets/7cc04ef7-fedf-4267-b08a-e7aadf2b0bd1)

bijum@MacBook-Pro dabproject % git remote add origin https://github.com/bijuthottathil/dabproject.git

bijum@MacBook-Pro dabproject % git push -u -f origin main


bijum@MacBook-Pro dabproject % git push -u -f origin main

Enumerating objects: 31, done.
Counting objects: 100% (31/31), done.
Delta compression using up to 14 threads
Compressing objects: 100% (25/25), done.
Writing objects: 100% (31/31), 7.88 KiB | 7.88 MiB/s, done.
Total 31 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/bijuthottathil/dabproject.git
 + 5d29ff1...8bff3e4 main -> main (forced update)
branch 'main' set up to track 'origin/main'.
bijum@MacBook-Pro dabproject % 

You can see all files are added to git repository

![image](https://github.com/user-attachments/assets/3551daf8-01fe-43e9-a0da-8c2046fdfe11)


Step 9: Triggering the GitHub Action CICD

Merge the changes from main brach to the dev branch. If DAB Deployment was successfully processed then pull the changes from dev to prod branch

This purpose , I will be creating a new Dev branch from Main

In Vs Code , I created dev branch from main

![image](https://github.com/user-attachments/assets/b2fea030-f10d-4948-a85c-a50b243c493b)



bijum@MacBook-Pro dabproject % git checkout dev

M       README.md
Already on 'dev'
bijum@MacBook-Pro dabproject % git add .

bijum@MacBook-Pro dabproject % git commit -m "modified read me file"                     
[dev cb6981f] modified read me file
 2 files changed, 1 insertion(+), 1 deletion(-)
 create mode 100644 .DS_Store
bijum@MacBook-Pro dabproject % git push origin dev

Enumerating objects: 35, done.
Counting objects: 100% (35/35), done.
Delta compression using up to 14 threads
Compressing objects: 100% (29/29), done.
Writing objects: 100% (35/35), 8.58 KiB | 8.58 MiB/s, done.
Total 35 (delta 5), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (5/5), done.
remote: 
remote: Create a pull request for 'dev' on GitHub by visiting:
remote:      https://github.com/bijuthottathil/dabproject/pull/new/dev
remote: 
To https://github.com/bijuthottathil/dabproject.git
 * [new branch]      dev -> dev
bijum@MacBook-Pro dabproject %

As soon as I changed file in dev branch, automatically deployment started

![image](https://github.com/user-attachments/assets/e1cd9bae-31c2-4e9b-ae12-120ff5ec3776)



![image](https://github.com/user-attachments/assets/0bc9fd3f-125d-4d5b-bb26-3fc6334dccfc)

![image](https://github.com/user-attachments/assets/4be5ec1b-446f-4a8b-869f-d4fcd8a66d85)

![image](https://github.com/user-attachments/assets/f7c3288b-5fcf-4502-a3af-09a83210db11)

![image](https://github.com/user-attachments/assets/ba34aa8b-fa04-412e-a19d-1797b059fcb4)

![image](https://github.com/user-attachments/assets/ddb06d5c-c77b-4806-a672-3437eb000903)

You can see newly adde notebook is deployed in dev

![image](https://github.com/user-attachments/assets/0582e5d2-66c1-43ac-ba06-7ab3c67779e8)


Step 9  #deployment to production

Opened prod workspace and created a new token

![image](https://github.com/user-attachments/assets/a92189f6-ad24-4797-9a1c-6245a61fec4a)


Now I will merge dev branch data to prod branch. So it should display once data is merged

prod branch created and deployed code using databricks bundle deploy -t prod

![image](https://github.com/user-attachments/assets/8b96e844-b68e-4a9e-ba39-9ab8998d401f)

![image](https://github.com/user-attachments/assets/5edab31e-d8b1-455a-9f75-c30afd11298a)





