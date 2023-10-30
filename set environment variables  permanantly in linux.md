**Set an Environment Variable in Linux Permanently**
If you wish a variable to persist after you close the shell session, you need to set it as an environmental variable permanently. You can choose between setting it for the current user or all users.

in any user
sudo vi .bash_profile

run the file 
source .bash_profile

1. To set permanent environment variables for a single user, edit the .bashrc file:

> sudo nano ~/.bashrc

2. Write a line for each variable you wish to add using the following syntax:

> export [VARIABLE_NAME]=[variable_value]

3. Save and exit the file. The changes are applied after you restart the shell. If you want to apply the changes during the current session, use the source command:

> source ~/.bashrc

4. To set permanent environment variables for all users, create an .sh file in the /etc/profile.d folder:

> sudo nano /etc/profile.d/[filename].sh

6. Save and exit the file. The changes are applied at the next logging in.
