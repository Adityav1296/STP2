# Blind OS Command Injection With Time Delay

## Description
 This lab contains a blind OS command injection vulnerability in the feedback function.

The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response.

To solve the lab, exploit the blind OS command injection vulnerability to cause a 10 second delay. 

## Solve

- After launching the challenge, I accessed the feedback function as is mentioned in the challenge description.
- There, I quickly filled in the details like username, email, etc. and sent the feedback. Using burp, I intercepted it.
- Now, the task was to cause a 10 second time delay which can be done using two commands: `sleep 10` and `ping -c 10 127.0.0.1`. Now the only task was to find the correct field in the request where I can put this command.
- First, I tried the `name` field and put the `sleep 10` command after it but nothing happened and the response came out instantly. Now, I tried the second field which was `email`. In that also, I placed the same command and forwarded the request. This time, the response showed a proper delay and I solved the challenge.

    ss
    ss