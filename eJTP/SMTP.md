Here's how you can send a fake email to the root user using both telnet and the `sendemail` command:

**Using Telnet:**

1. Open a terminal or command prompt.

2. Use telnet to connect to the SMTP server on port 25:

   ```
   telnet <SMTP_SERVER_IP> 25
   ```

3. Once connected, initiate the SMTP conversation:

   ```
   EHLO example.com
   ```

4. Send the "MAIL FROM" command to specify the sender's email address:

   ```
   MAIL FROM: <sender@example.com>
   ```

5. Send the "RCPT TO" command to specify the recipient's email address (root user in this case):

   ```
   RCPT TO: <root@localhost>
   ```

6. Start the data input by sending:

   ```
   DATA
   ```

7. Enter the email content, including the subject, message body, and any other headers, followed by a period on a line by itself to indicate the end of the email:

   ```
   Subject: Fake Subject
   From: Fake Sender <sender@example.com>
   To: Root User <root@localhost>

   This is a fake email.
   .
   ```

8. To exit, send:

   ```
   QUIT
   ```

**Using sendemail Command:**

1. Make sure you have the `sendemail` command installed. If not, install it using the appropriate package manager for your system.

2. Use the following command to send a fake email to the root user:

   ```
   sendemail -f sender@example.com -t root@localhost -u "Fake Subject" -m "This is a fake email."
   ```

Replace `sender@example.com` with the sender's email address and adjust the email content as needed.

Remember that these actions should only be performed in a controlled lab environment for educational purposes and with proper authorization. Sending fake emails or engaging in any unauthorized activities is against ethical guidelines.