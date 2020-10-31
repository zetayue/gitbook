# Connect to HPCs

The IP addresses of the [DGX-Lei workstation](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/summary-of-hpcs/dgx-lei-workstation) and the [Hunter DLS Server](https://compsci-hunter.gitbook.io/xie-research-group/hpc-environments/summary-of-hpcs/hunter-dls-server) are:

| HPC | IP address |
| :--- | :--- |
| DGX | 146.95.214.135 |
| DLS | 146.95.128.169 |

Both of the two machines are inside the network of the Hunter College \(Hunter network\).

## 1. Inside the Hunter Network

You can use `ssh` via a computer inside the Hunter network to access it:

```text
ssh yourname@IPaddress
```

## 2. Outside the Hunter Network

If you want to access the HPCs outside the Hunter network, you can use the following two ways:

### 2.1. SSH with Username and Password

1. Use `ssh` to connect to eniac from anywhere using your [eniac account](http://web.archive.org/web/20190726111206/http://www.geography.hunter.cuny.edu/tbw/CS.Linux.Lab.FAQ/department_of_computer_science.faq.htm):

   ```bash
   ssh username@eniac.cs.hunter.cuny.edu
   ```

   \(Note: To apply a new eniac account or reset your password, just send an email to [cstechsp@hunter.cuny.edu](mailto:cstechsp@hunter.cuny.edu) with your Hunter email address\)

2. After connecting to eniac, use `ssh` to connect to the HPCs:

   ```bash
   ssh username@IPaddress
   ```

### 2.2. SSH with SSH Key Pair \(Recommended\)

This way is more secure than the first one and you don't need to type your password every time.

1. Create your own ssh keys  
  
   **MacOS/Linux:**

   ```bash
   ssh-keygen
   ```

   Hit enter to keep the default settings all the way util the key pair is successfully created.  
  
   **Windows:**  
   Windows now support OpenSSH. If `ssh` command does not work, you should be able to install it following the instructions here: [OpenSSH in Windows](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_overview). The following protocols are based on OpenSSH. You should also be able to do it with PuTTY, but it is not recommended.  

2. Copy your public key to eniac and HPCs  
  
   After generating ssh key pair, you should get two files in .ssh directory: id\_rsa and id\_rsa.pub. id\_rsa is your private key, you should keep it only to yourself. The id\_rsa.pub file is the public key file. We need to copy it to eniac and HPCs so we do not need to type password every time. The command to copy your id\_rsa.pub to other computer is:

   ```bash
   ssh-copy-id -i ~/.ssh/id_rsa.pub user@host
   ```

   The copying will ask for your password. You need to copy your public key to eniac first, connect to eniac with ssh, copy the same key to HPCs \(DGX or DGL\) again. Now you should be able to connect to eniac with

   ```bash
   ssh username@eniac.cs.hunter.cuny.edu
   ```

   and connect to HPCs with

   ```bash
   ssh username@IPaddress
   ```

   without being asked for passwords.  

3. Set up ssh config file  


   Setting up a ssh config file will let you connect to HPCs from your local machine with only one line of command without complex url or ip addresses. First, you need to create a file named config in .ssh folder. In the file, put the following contents:

   ```bash
   Host jump-box
      HostName eniac.cs.hunter.cuny.edu
      User eniac_username
      IdentityFile ~/.ssh/id_rsa (path/to/your/id_rsa)
      ServerAliveInterval 240

   Host dgx
      HostName 146.95.214.135
      User dgx_username
      IdentityFile ~/.ssh/id_rsa (path/to/your/id_rsa)
      ProxyCommand ssh -q -W %h:%p jump-box
      ServerAliveInterval 240

   Host dls
      HostName 146.95.128.169
      User dls_username
      IdentityFile ~/.ssh/id_rsa (path/to/your/id_rsa)
      ProxyCommand ssh -q -W %h:%p jump-box
      ServerAliveInterval 240
   ```

   The first code block defines a proxy host. In our case, it is the eniac server. The second and third code blocks are for DGX and DLS, respectively. In the case of Windows, if the ProxyCommand causes errors, you can try to replace the `ssh` with the full path to `ssh.exe` file. For example, in my case, the ProxyCommand is:

   ```bash
   ProxyCommand C:\\Windows\\System32\\OpenSSH\\ssh.exe -q -W %h:%p jump-box
   ```

   ServerAliveInterval variable lets your terminal send an empty package to the server every x seconds to keep your connection alive. With the config file, you should be able to connect to HPCs simply with:

   ```bash
   ssh dgx
   ```

   or

   ```bash
   ssh dgl
   ```

   You should also be able to use `scp` command as below:

   ```text
   scp local/file.txt dgx:/raid/home/username/...
   ```

