# Setting up 2FA with Google Authenticator on Ubuntu 18.04 - that actually works!

I tried to configure  2FA with Google Authenticator using the step-by-step instructions from here: 
https://ubuntu.com/tutorials/configure-ssh-2fa

However, after following all the steps, I could not login using SSH at all - after entering the correct password, I got presented with a *permission denied* error, without even requesting the OTP. Thanks goodness for the VPS  provider console!

Below are the steps that helped me to resolve this issue:

1. Remove password as a supported authentication method and rely on keyboard-interactive for your password authentications. Set "PasswordAuthentication no" in /etc/ssh/sshd_config

So: /etc/ssh/sshd_config
```
UsePAM yes
PasswordAuthentication no
ChallengeResponseAuthentication yes
```

2. Ensure both UNIX authentication and Google Authenticator are included in /etc/pam.d/sshd **in that order!**

```
auth    required      pam_unix.so     no_warn try_first_pass
auth    required      pam_google_authenticator.so
```

Otherwise you will get permission denied errors!

3. Now, when you login using SSH, you should get your usual password prompt first, and then the OTP request 
