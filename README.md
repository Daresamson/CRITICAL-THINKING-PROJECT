# Project Title: Automated Server Update and Web Server Installation Script
## Shell Script Example

#!/bin/bash

 Automated Server Update and Web Server Installation Script
 Author: [Your Name]
 Date: [Current Date]
 Purpose: To update multiple remote servers and install Nginx.

 Function to print usage information
usage() {
    echo "Usage: $0 [server1] [server2] ... [serverN]"
    echo "Example: $0 server1.example.com server2.example.com"
    exit 1
}

## Function to update and install packages
update_and_install() {
    local server=$1
    echo "Connecting to $server..."

    ## SSH command to perform update and install Nginx
    ssh -o BatchMode=yes "$server" << 'ENDSSH'
        ## Update the package list
        echo "Updating package list..."
        sudo apt-get update -y || { echo "Failed to update on $HOSTNAME"; exit 1; }

        ## Upgrade installed packages
        echo "Upgrading installed packages..."
        sudo apt-get upgrade -y || { echo "Failed to upgrade on $HOSTNAME"; exit 1; }

        ## Install Nginx if not installed
        if ! command -v nginx &> /dev/null; then
            echo "Installing Nginx..."
            sudo apt-get install nginx -y || { echo "Failed to install Nginx on $HOSTNAME"; exit 1; }
        else
            echo "Nginx is already installed on $HOSTNAME."
        fi

        echo "Update and installation complete on $HOSTNAME."
ENDSSH

    # Check the exit status of the SSH command
    if [ $? -ne 0 ]; then
        echo "Error connecting to $server. Please check your SSH configuration."
    fi
}

## Main script execution
if [ "$#" -lt 1 ]; then
    usage
fi

## Loop through each server provided as an argument
for server in "$@"; do
    update_and_install "$server"
done

echo "All operations completed."






# Explanation of Key Components
Usage Function: Displays how to use the script and exits if no servers are provided.

SSH Command: Uses a here-document to send multiple commands to the remote server. This ensures that all commands run in a single SSH session, reducing connection overhead.

Error Handling:

Each command is followed by a check for success. If a command fails, an informative message is printed, and the script exits with a non-zero status.
The SSH command itself is also checked to ensure the connection was successful.
Parameterization: The script takes server addresses as command-line arguments, allowing for flexible use.

User-Friendly Output: The script echoes messages that inform the user about what operations are being performed.

Documentation: Comments throughout the script explain the purpose of each section, enhancing readability and maintainability.

Testing and Documentation
Testing: Before running the script in production, test it in a safe environment with virtual machines to ensure it behaves as expected across different versions of Ubuntu.
Documentation: Create a user guide that explains how to run the script, required permissions, and what to expect from the output.
