title: "sample code of sending email using amazon SES on .NET framework/c#"
tags:
  - amazon
id: 33
categories:
  - code snippets
date: 2012-04-15 08:29:57
---

Here is a c# code sample of sending simple email message using amazon Simple Email Service webservice api.
```c#
        public void SendMail(String msg, String receiver)
        {
            Thread thread = new Thread(new ParameterizedThreadStart(SendEmail));
            string[] mail = new string[] { receiver,msg};
            thread.Start(mail);

        }

        public  void SendEmail(object mail)
        {
            string[] m = (string[])mail;
            //Set up the client with your access credentials
            var client = new AmazonSimpleEmailServiceClient(Config.GetValue("AWSID"), Config.GetValue("AWSKEY"));
            SendEmailRequest req = new SendEmailRequest()
                .WithDestination(new Destination() { BccAddresses = new List&lt;string&gt;{ m[0]} })
                .WithSource(Config.GetValue("EMAILSender"))
                .WithReturnPath(Config.GetValue("EMAILSender"))
                .WithMessage(
                new Amazon.SimpleEmail.Model.Message(new Content(m[1]),
                new Body().WithText(new Content(m[1]))));

                var resp = client.SendEmail(req);
        }
```c#