#### Running Google OR tools on AWS
I work primarily on scheduling problems which are [NP-Hard](_posts/2021-11-08-Job shop scheduling in python.md). As the size of the input increases the time to find a optimal or even a 
feasible solution increases. My workstation is remarkably under-powered to test out larger instances of the job shop scheduling problems. So I am goinf test the input data out on 
AWS free-tier. Hopefully I can convince my bosses that powerful workstations have a good return on investment! <br>

Before deploying anything on AWS, I googled "google or tools on aws". This popped up only two different pages but both were related to AWS Lambda. I don't know anything about AWS Lambda 
and am going to stick to EC2 instances which I have used in the past. <br>

#### Launching an instance on AWS EC2
[OR-Tools](https://github.com/google/or-tools) has been tested on Ubuntu 18.04 LTS and up (64-bit) so we I going to to launch a Ubuntu Server 20.04 LTS (HVM).
<br>
##### Step 1: Choose an Amazon Machine Image (AMI) <br>
Ubuntu Server 20.04 LTS (HVM), SSD Volume Type <br>
##### Step 2: Choose an Instance Type 
t2.micro is eligible for free tier
<br>
The other steps I am leaving to default. One thing to keep in my mind is that to SSH into an instance we need the security key. If you have an existing key you can use that else
you need to create a new security key. It should be private. <br>

#### Connecting to an AWS EC2
There are two options to connect to a EC2 instance from a Windows machine: [puTTY](https://www.putty.org/) and [Powershell](https://docs.microsoft.com/en-us/powershell/scripting/overview?view=powershell-7.2).
I am following the tutorial on [cloudlinuxtech](https://cloudlinuxtech.com/ssh-to-ec2-instance/) to connect to the EC2 instance. <br>
First the .pem key has to be converted to a .ppk key that putty understands. This is done using the putty key generator.
Then you can connect to the instance using the ppk key. The username is my-instance-user-name@my-instance-public-dns-name. <br>
One mistake I made is that I configured the security key so that traffic from a specific IP address could connect to the instance. This can be changed by allowing all IP adress in the 
scurity group settings. <br>

#### Dr. Debugging or: How I Learned to Stop Worrying and Love the Bugs
The first error I got when trying to run the script was that the model was invalid. ortools version -  9.2.9972 <br>
Solve status: MODEL_INVALID <br>
After some googling I found a issue on the ortools github page where the [model was invalid on windows](https://github.com/google/or-tools/issues/1365). It appears that there is some issue with the format of integers. There is piece of code in my script where I am subtracting two datetime columns in pandas and I suspect where Int32 is being introduced  

