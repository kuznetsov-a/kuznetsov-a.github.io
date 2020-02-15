#Setting up 2FA with Google Authenticator on Ubuntu 18.04 - 

I tried to configure  2FA with Google Authenticator using the step-by-step instructions from here: 
https://ubuntu.com/tutorials/configure-ssh-2fa

1. Remove password as a supported authentication method and rely on keyboard-interactive for your password authentications. Set "PasswordAuthentication no" in /etc/ssh/sshd_config

So: /etc/ssh/sshd_config

UsePAM yes
PasswordAuthentication no
ChallengeResponseAuthentication yes

2. Ensure both UNIX authentication and Google Authenticator are included in /etc/pam.d/sshd in that order!

auth    required      pam_unix.so     no_warn try_first_pass
auth    required      pam_google_authenticator.so


Otherwise you will get permission denied errors!

3. Now, when you login using SSH, you should get your usual password prompt first, and then the OTP request 
