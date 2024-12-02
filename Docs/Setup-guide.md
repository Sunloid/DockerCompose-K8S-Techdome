# Setup for the Project 
This file contains all the information needed for setting up the project. 

## AWS Instance 
The main problem I ran into while developing this project was the disk space. Docker files  which include images, containers, volumes, build data and cache require a large amount of space. There are two ways of dealing with this one using a instance type which features a huge amount of space (t2.large) or using a small instance (t2.micro) and installing a EBS volume on the same. 

The easier method would be to use a instance which huge amount of memory like t2.large . For security enable all traffic and use ubuntu linux. 
 ![Alt text](<image (26).png>)