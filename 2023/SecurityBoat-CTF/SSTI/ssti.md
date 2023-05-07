# Challenge

In this CTF challenge, we were presented with a registration page that appeared to be vulnerable to Server-Side Template Injection (SSTI). The goal of the challenge was to exploit the SSTI vulnerability and print the flag.

## Identifying the Vulnerability

To confirm whether the registration page was vulnerable to SSTI, we intercepted the request using Burp and injected the following payload into the username parameter:
``
{{7*7}}
``
If the server responded with 49, it would confirm that the registration page was vulnerable to SSTI. In our case, the server did respond with 49, indicating that the SSTI vulnerability existed.
## Identifying the Template Application

The next step was to identify the template application that was being used on the server. To do this, we injected the following payload into the username parameter:
``
{{1/0}}
``
This caused an error that revealed the Twig installation files path, which helped us to identify the template application being used.

## Exploiting the Vulnerability

With the SSTI vulnerability confirmed and the template application identified, we were ready to exploit the vulnerability to print the flag. We used the passthru function to execute a command from an input array using a filter. Our final payload looked like this:
``
%7b%7b['rev+../home/flag']%7cfilter('passthru')%7d%7d
``
This payload allowed us to navigate to the home directory and print the flag in reverse order using the rev command. we used rev because cat command was blacklisted.

## flag
``
Here is your flag {SSTI_4nd_t3mpl4t3s_4r3_fun}
``
