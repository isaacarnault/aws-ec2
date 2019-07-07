### EC2
- `$ https://console.aws.amazon.com.` // log into your `AWS` management console<br>

I'm using `MFA` to secure my root account access coupled with `Google Authenticator` on my `Android` smartphone.<br>

You can bypass this step and login normally.<br>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS-1.jpg](https://i.postimg.cc/L5F2KQwp/isaac-arnault-AWS-1.jpg)](https://postimg.cc/nj26q2nR)

</p>
</details>

- Go to Services > EC2<br>

- In "Create Instance" section, click on "Launch Instance"<br>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS2.png](https://i.postimg.cc/nVSG28yg/isaac-arnault-AWS2.png)](https://postimg.cc/6TRZ6P5f)

</p>
</details>

- Select Amazon Linux 2 AMI (HVM), SSD Volume Type<br>

- Instance type: choose t2.micro (Free tier eligible). Instance comes with 1vCPU and 1 GiB (memory).<br>

- Click on "Next: Configure instance details"<br>

- Configure instance details : leave all fields as they're by default, just Enable termination protection.<br>

<details>
<summary>🔴 See output</summary>
<p>  

[![isaac-arnault-AWS3.png](https://i.postimg.cc/Sx69wHPy/isaac-arnault-AWS3.png)](https://postimg.cc/mPrh9pRq)

</p>
</details>

- Click on "Next : Add Storage". Leave default configuration then click on Next: Add Tags. You can leave tags blanks, here I'm using some tags for my own needs.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS4.png](https://i.postimg.cc/TY8qFjPJ/isaac-arnault-AWS4.png)](https://postimg.cc/8sH6r6M7)

</p>
</details>

- Click on "Next : Configure Security Group". Click on "Add rule" to allow `Port 80` on `Http.` Ignore the warning and click on "Review and Launch" then "Launch".

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-5.png](https://i.postimg.cc/0QwK037R/isaac-arnault-AWS-5.png)](https://postimg.cc/5YftxsPn)

</p>
</details>

- Now you are about to create a key pair to securely `SSH` into your instance. Select "Create a new key pair", name it as you want then "Download Key Pair". This should download a .pem file. Mine is MYKP1.pem.

- Create a new folder named "SSH" on your "Desktop", then move the .pem file to the newly created folder.

- Change the permissions to .pem file, ie: $ chmod 400 MYKP1.pem.<br>

- Connect to your `EC2` instance using your `IPv4 Public IP Address` provided by your `EC2` instance: $ ssh ec2-user@myipv4.public.address -i MYKP1.pem. Then type "yes" when prompted by the `CLI`. See screenshot below if everything worked fine.<br>

Now, you should be connected to your `EC2` instance through `SSH`.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-9.png](https://i.postimg.cc/SxvYw8LG/isaac-arnault-AWS-9.png)](https://postimg.cc/nXqLmXPs)

</p>
</details>

<hr>

## Important

- If connection failed, you are probably blocked by a proxy (you are trying to connect from a Public Libraby for example) not allowing your device using `Port 22`, which is the default port used by the `SSH`.<br>

<hr>

## Another method : Secure Shell App (Web)

- Now we are going to use another method if you want to connect to your `EC2` instance, if you can't / do not want to use your `CLI`. We are going to use `Secure Shell App` extension in our `Chrome` browser.<br>

Search for "Secure Shell App" in your Search Engine and Download it as below :<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-10.png](https://i.postimg.cc/3rp4VPVd/isaac-arnault-AWS-10.png)](https://postimg.cc/F1rHJn94)

</p>
</details>

- Once installed, you should see this. Then click on "Secure Shell App" icon.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-11.png](https://i.postimg.cc/0QpfkZcv/isaac-arnault-AWS-11.png)](https://postimg.cc/FdHcGyMC)

</p>
</details>

- If everything goes well, enter the following information into your Secure Shall App: your username (ec2-user) and your hostname (provided by your EC2 instance).<br>

Go in the SSH folder where you stored the .pem file from your CLI and generate a public key from the .pem, using : $ ssh-keygen -y -f MYKP1.pem > MYKP1.pub.<br>

- Now you should have 2 files, MYKP1.pem and MYKP1.pub. Duplicate "MYKP1.pem" file and rename the new file to "MYKP1" (remove .pem extension). Regarding the "Identity section", upload both the "MYKP1.pem" file as well as a copy of that file without the .pem extension (MYKP1).

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-12.png](https://i.postimg.cc/D0YMqShw/isaac-arnault-AWS-12.png)](https://postimg.cc/8fM4GPs2)

</p>
</details>  

- There you go, if everything went fine you should be prompted by the Secure Shell App, type "yes" and you should see the same display as you would normally see in your CLI.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS.png](https://i.postimg.cc/85xPkBX1/isaac-arnault-AWS.png)](https://postimg.cc/5YqMnvMG)

</p>
</details> 

<hr>

- Let's get back to your installation using your `CLI`. The following steps are not performed in `Secure Shell App`, but you can perform them there.<br>

- Elevate your privileges to root using : $ sudo su and perform $ yum update -y to update your `CLI` with the latest available packages.<br>

- Install `Apache HTTP Server` from your `CLI`. This will basically turn your `EC2` instance to a web server : use $ cd /var/www/html to make your web server's files accessible by `Port 80`.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-13.png](https://i.postimg.cc/4xPRdb2z/isaac-arnault-AWS-13.png)](https://postimg.cc/Snnv18hs)

</p>
</details>

- Let's create a sample index.html file using $ nano index.html<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-14.png](https://i.postimg.cc/sg8GtsjG/isaac-arnault-AWS-14.png)](https://postimg.cc/nX4Lm8Qn)

</p>
</details>

- To start the httpd service, use $ service httpd start.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-15.png](https://i.postimg.cc/k4NV6SxW/isaac-arnault-AWS-15.png)](https://postimg.cc/xJCjBcpd)

</p>
</details>

- There you go, type your IPv4 public address in your browser and you should see your web server online.<br>

<details>
<summary>🔴 See output</summary>
<p>

[![isaac-arnault-AWS-16.png](https://i.postimg.cc/8cSZMxjs/isaac-arnault-AWS-16.png)](https://postimg.cc/k6TNd1Nd)

</p>
</details>
