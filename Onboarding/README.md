# Use CloudCheckr's API to Onboard Users and Groups

The purpose of this script is to provide a process for creating users and groups with the API without having to directly interact with user ids or group ids. All of the required information is contianed in the csv input files.

---

## Steps to Create Users and Groups


1. Create a group that will act as a permission template through the user-interface.
2. Run the python script GetAcls/get_access_control_list_per_group.py to download the permissions of the permission template. Each account type will have its own set of permissions. There are four account types Aws Account, Aws MAV, Azure Account, and Azure Mav. This will export up to four csv files on permissions. These files should be copied to the AddGroupPermissions folder.
3. Run the python script CreateGroups/create_groups.py to create the groups. This python script will first check if there are duplicate groups and stop running if there are.
4. Run the python script in AddGroupPermissions/add_group_permissions.py to add permissions to the groups that were created.
5. Run the python script in AddUsers/add_users.py to add users to CloudCheckr.
6. Run the python script in AddUsersToGroups/add_users_to_groups.py to add users to groups.


---

## How to Create an Admin API Key

1. Log in as an admin user through the UI.
2. Go to Admin Functions on the upper right hand corner and click Admin Api Key.
3. Click create admin key.


---

## How to edit the input csv files

THe CloudCheckr API for users and groups has numerous places where it requires UUID strings that correspond to users or groups. By using this program and the associated csv files, you can erase the need for manually downloading these as this program will find the UUID based on the group name.

1. Navigate to the folder corresponding to the program that you want.
2. The first column will always be the environment. In most cases this will be "https://api.cloudcheckr.com".
3. The following columns will be the user or group names.

---

## How to run a python program

1. All of the python programs can be run through the command line. They will have the name of the input csv file hard coded in. The input csv file should be editted and updated before running the python program.
2. The python version used is python 3.6.5. The python libraries used are logging, numpy, pandas, and requests. These can be installed with pip.
3. You should navigate to the python program and run python script_name.py <admin-api-key>
4. The Admin Api Key will be a 64 character string that will be generated by CloudCheckr.

---

## How to add a new environment such as a SelfHosted version of CloudCheckr

Most customers will use either api, eu, or au for the environment. If you use are using a custom environment, then this should be added. This program will check if the environment is valid before running, so if a custom environment is used it should be added to the program.

Every individual program will have a function called check_invalid_env. This should be editted to include the new environment such as https://selfhosted.cloudcheckr.com.

---

## Duplicate Groups Check

In the past, Cloudcheckr has allowed for two groups to have the same name. Improved name validation was added to prevent users from creating duplicate group names with the API or the UI. Some customers still have two groups with the same name and they will continue to work. However before any change or update to the groups can be put made, we recommend re-naming groups to remove the duplicate group names.

This program will check if there are duplicate group names and prevent any further action until duplicate group names have been removed. This is because there are many places were unique group names are used to identify groups in this program.

---

## Logging

Every update to CloudCheckr in this program will be done with a post request to the CloudCheckr API. This action will be logged in two places. The first is to the command line with python's native print statements. The second is to a log file that uses python's native log.

This program will include example logs when it is first downloaded as a reference. These should be cleared out if you want to start fresh.

---

## CloudCheckr Group ACLs Info

Every group permission is defined by a particular ACL (access control list). This permission will be unique to the kind of account it is. There are four kinds of accounts Aws Accounts, Aws MAVs, Azure Accounts, and Azure MAVs.

The access control list is a long string such as 536f7e9a-c15f-4952-a289-e870f3a09930[CC_Delimiter]07ac5ec8-c453-494a-b7ed-469e3090b2b5. This string will correspond to the permission Generic,See List Costs.

We recommend setting up a template group through the UI and logging in as a test user through that group to see if the permissions align with what you expect. Then downloading the ACLs from this template group to expand to other groups. This way you can decrease how much you directly interact with these ACLs.