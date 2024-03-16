# anisble-integration

# Create the ssh-key using the below command.
ssh-keygen -t ed25529 -C "Ansible SSH key"      // -t : type of the SSH key // -C : comment

# Copy the generated public key into the destination host.
ssh-copy-id -i ~/.ssh/ansible_key.pub workstation-01.server.local    // -i :  input file then name of the key file.
[ After executing this command, can find the content of the file under the "authorized_keys" file on the target host ]

#  