# Error/Challenges Logs 

The main code for the frontend and backend was taken from different repository. 

- FrontEnd: https://github.com/Anand-1432/Techdome-frontend

- BackEnd: https://github.com/Anand-1432/Techdome-backend

I made a few changes in the code after I forked them for my account. 

1. **dotenv**: The .env file was missing which contained necessary environment varibles for the project such as the 
    - PORT 
    - SECRET
    - Necessary Credentials for Cloudinary 

2. **Header**: There was a typo in userAction and blogAction files in the frontend repository. 

<>

<>

3. **No space left on disk**: The biggest problem I faced was the space issue. Free instances like the t2.micro was not enough to run the entire application. I had  to switch to t2.large and attach a EBS volume to the instance. 

4. **API**: I had messed up the api calls in the code during the implementation. I solved the issue by pulling the code into VS code and used the developer tools to pinpoint the error. 