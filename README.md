# anisble-integration

# Create the ssh-key using the below command.
ssh-keygen -t ed25529 -C "Ansible SSH key"      // -t : type of the SSH key // -C : comment

# Copy the generated public key into the destination host.
ssh-copy-id -i ~/.ssh/ansible_key.pub workstation-01.server.local    // -i :  input file then name of the key file.
[ After executing this command, can find the content of the file under the "authorized_keys" file on the target host ]

# Execute the add-hoc commands | No changes
ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m ping           // -m : module [ping module]
[Test the connectivity to the target hosts]

ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m gather_facts   // gather_facts module
[Gather fatcs of target hosts]

# Execute the add-hoc commands | With changes
ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m dnf -a update_cache=true   // -a : argivement
[Passing the update_cache argivement witht the dnf module]

ansible all -i inventory/hosts --private-key=$ANSIBLE_SSH_KEY -m dnf -a "name=httpd state=present" --become-user=jenkins --become-method=sudo