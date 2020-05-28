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

# Importing notebooks from Google Colab into your own Jupyter environment

Sometimes you start working on a project in Colab and then need to bring it into your own Jupyter environment.

In case if you have some complex visualisations, or just long outputs, you can  get an error like below
```
notebook validation failed: {'model_id': '44ec700bca7d481f94e3fbde738840c0', 'version_major': 2, 'version_minor': 0}
```

Just clear all outputs before saving the notebook again - the error will go away.

# Handling conflicting packages requirements when installing random libraries from GitHub

1. First try installing with conda/pip in the desired environment
1.1 When it fails due to conflicting packages requirement, review the installation logs to see what packages are clashing
2. Clone the library from GitHub to your machine
```
git clone https://github.com/*
```
3. Find the cloned repo folder and edit *requirements.txt*
 *A rule of a thumb is always change to the higher of the required version, as it is more likely to work*
4. **Important** Ensure the right conda environment is activated and your path points to the right pip
``` which pip ```
5. Go to the cloned repo folder and install the "hacked" library
```
cd your-local-repo
pip install -e .
```
