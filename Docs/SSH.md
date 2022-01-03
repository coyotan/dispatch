# <h1 align="center">Configuring SSH</h1>

## What is SSH?
According to Wikipedia:

Secure Shell (SSH) is a cryptographic network protocol for operating network services securely over an unsecured network. Typical applications include remote command-line, login, and remote command execution, but any network service can be secured with SSH. 

To summarize, SSH is a protocol that provides a secure, reliable, and encrypted method of communication between two hosts.

## Why SSH?
Transmission Control runs separate from the server(s) it deploys torrent containers on. This enables a greater degree of control and better access to scalability. 

In order to deploy containers on a remote docker instace we must establish a secure channel of communications. For this we can use either TLS or SSH. We've elected to use the SSH approach, as the TLS method introduced some complexities into the configuration process which may make Transmission Control less user-friendly during the configuration process.

## How do I configure SSH for Transmission Control?
Before moving to the SSH configurations, make sure that the server you intend to deploy Transmission Control is able to connect to the target Docker server via SSH normally. If it is not able to connect to the server under normal conditions, you will need to troubleshoot these connection issues before continuing.

## All Operating Systems

The Windows, Linux, and macOS configurations vary only slightly from each other. Please carefully read the instructions below to configure SSH for Transmission Control on your OS.
<br><br>
Just a reminder to carefully and completely read the instructions before pressing enter.<br>
["The warnings come after the spells"](https://youtu.be/S8r8RAkLuz0?t=225)
<br><br>
Some versions of Windows support an optional feature that provides OpenSSH. This can be enabled by going into Windows Settings, Apps & Features, Optional Features, and enabling the OpenSSH feature if it is not already enabled. This must done as Docker relies on this service to connect to the remote server.

After completing the above step, install Docker.
<br><br>
<b>Note</b>: You do not need to install the WSL2 package for Docker for Windows. We will use Docker Context to connect to a remote docker server. Transmission Control does not currently support deploying docker containers on the same server it is deployed on.
<br><br>
Run the following commands in an Administrator instance of PowerShell.
```bash
# Generate a new SSH key, also known as an Identity.
ssh-keygen

# Change into the directory the file was generated in.
cd $env:USERPROFILE\.ssh

# For Linux/macOS users
cd ~\.ssh

# Copy the public key to the server.
# You will need to change the username and remote_hostname to match your environment.
type id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# For Linux/macOS users
cat id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# If prompted, type yes, then enter the password to the account you're attempting to connect to.

# Now that we've established our identity, we should be able to conenct to the server without a password.
ssh username@remote_host

# If you were not prompted for a password, congratulations! You have successfully configured SSH for Transmission Control. You may proceed into the Docker Section.

# If you were prompted for a password, you will need to troubleshoot the connection issue. Please refer to the documentation from Digital Ocean for more information.

# <--- Docker Section --->

# Create a Docker Context for Transmission Control to use the remote Docker environment.
docker context create --docker host=ssh://username@remote_host --description="Context for Transmission Control Remote Docker Connection" transmission-control

# Select the new docker context.
docker context use transmission-control

# Test the new docker context by getting information from the Docker server.
docker info

# If you were prompted for a password, you will need to troubleshoot your ssh agent. If the issue persists, using SSH Host Configs is also supported by docker. You will need to change the host=ssh://username@remote_host to the name of the host config in your .ssh/config file. if you take this approach.

# If you were not promtped for a password, congratulations! You have configured Docker and SSH for Transmission Control.
```

## Additional Troubleshooting

Some information about SSH Configurations can be found here: [DigitalOcean SSH Guidance](https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server)