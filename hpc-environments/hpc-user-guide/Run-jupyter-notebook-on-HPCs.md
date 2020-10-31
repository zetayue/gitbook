# Run Jupyter server with GPU access on HPCs

You can run a Jupyter notebook/lab server that has GPU access inside your container and connect to it from your local computer.

### 1. Set up `ssh` connection with DGX or DLS as describt in [Connect to HPCs](connecting-to-hpcs.md)
After completing the set up process, you should be able to connect to DGX or DLS with a single line of command:
```bash
ssh dgx
```
or
```bash
ssh dls
```

### 2. Add port forwarding
In your ssh config file, inside dgx and dls blocks, add the following line:
```text
LocalForward 56789 127.0.0.1:56789
```
To avoid conflict with other people and other applications, the port number 56789 should be replaced by a port number in [49152, 65535]. The modified config file should be similar to this:
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
      LocalForward 56789 127.0.0.1:56789
      ServerAliveInterval 240

   Host dls
      HostName 146.95.128.169
      User dls_username
      IdentityFile ~/.ssh/id_rsa (path/to/your/id_rsa)
      ProxyCommand ssh -q -W %h:%p jump-box
      LocalForward 56789 127.0.0.1:56789
      ServerAliveInterval 240
```
### 3. Start container with GPU port published to host
Add an additional `-p` argument when starting your container:
```bash
nvidia-docker run --name containerName --network=host -p 56789:56789 -it -v localDir:containerDir imageRepository:tag
```

### 4. Insider your container, start Jupyter server

Start your Jupyter notebook/lab server with:
```bash
jupyter notebook --ip 0.0.0.0 --port 56789 --allow-root
```
or
```bash
jupyter lab --ip 0.0.0.0 --port 56789 --allow-root
```
To run the server in background, so you can still do other things within your container, start the server with `nohup`:
```bash
nohup jupyter notebook --ip 0.0.0.0 --port 56789 --allow-root > jupyter.log &
```
or
```bash
nohup jupyter lab --ip 0.0.0.0 --port 56789 --allow-root > jupyter.log &
```
Notice the `&` symbol at the end of the commands.

### 5. Connect to the server
On your local machine, type `localhost:56789` in your browser address bar and hit enter. If this is your first connection, you will see a page similar to:

<p align="center">
  <img height=170px width=600px src="../../resources/images/jupyter_login_page.png">
</p>

You can find the token in your jupyter.log file. The token should look like this:
<p align="center">
  <img height=70px width=550px src="../../resources/images/jupyter_token.png">
</p>
Copy and paste the token into the text bar and click log in. Now you should be able enjoy the convenience of Jupyter notebook/lab on our DGX/DLS with the power of GPUs.

### 6. (Optional) Set up a password for your Jupyter notebook
If you do not like the long token string, you can set up a password for your notebook. To do that, before starting the server, you need to create a config file for Jupyter:
```bash
jupyter notebook --generate-config
```
then create password:
```bash
jupyter notebook password
```
Type in your password twice. Then an encrypted password with be saved into the config file you created above. Now your can start the Jupyter sever as in step 4. When you connect to the server started this way, the login page will only ask you for the password.
