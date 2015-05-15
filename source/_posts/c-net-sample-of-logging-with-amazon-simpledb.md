title: "c#/.net sample of logging with amazon simpleDB"
tags:
  - amazon
id: 14
categories:
  - code snippets
date: 2012-04-12 14:35:12
---

The following is a thread safe, async logger written in c#, with amazon simpleDB backed.
The log will be saved to a domain named "fis-log".
Every log entry will be processed in different threads. So it will have minimum performance impact. 

```
public class SDBBasedLogger : ILogger
    {
        private AmazonSimpleDB sdb;
        public SDBBasedLogger()
        {
            bool domainExists = false;
            sdb = AWSClientFactory.CreateAmazonSimpleDBClient(Config.GetValue("AWSID"),
                  Config.GetValue("AWSKEY"), 
                  new AmazonSimpleDBConfig().WithServiceURL("https://sdb.ap-northeast-1.amazonaws.com/"));

            ListDomainsResponse sdbListDomainsResponse = sdb.ListDomains(new ListDomainsRequest());
            if (sdbListDomainsResponse.IsSetListDomainsResult())
            {
                ListDomainsResult listDomainsResult = sdbListDomainsResponse.ListDomainsResult;
                if (listDomainsResult.DomainName.Contains("fis-log"))
                {
                    domainExists = true;
                }
            }

            if (!domainExists)
            {
                CreateDomainRequest createDomain = (new CreateDomainRequest()).WithDomainName("fis-log");
                sdb.CreateDomain(createDomain);
            }
        }

        public void Clear()
        {
            DeleteDomainRequest req = new DeleteDomainRequest().WithDomainName("fis-log");
            ListDomainsResponse sdbListDomainsResponse = sdb.ListDomains(new ListDomainsRequest());
            if (sdbListDomainsResponse.IsSetListDomainsResult())
            {
                ListDomainsResult listDomainsResult = sdbListDomainsResponse.ListDomainsResult;
                if (listDomainsResult.DomainName.Contains("fis-log"))
                {
                    lock (sdb)
                    {
                        sdb.DeleteDomain(req);
                        CreateDomainRequest createDomain = (new CreateDomainRequest()).WithDomainName("fis-log");
                        sdb.CreateDomain(createDomain);
                    }
                }
            }
        }

        public void Info(string msg)
        {
            LogData ld = new LogData();
            ld.Message = msg;
            ld.Level = "info";
            ld.Category = "";
            ld.Time = DateTime.Now.ToString("yyyyMMddHHmmssffff");
            ThreadPool.QueueUserWorkItem(new WaitCallback(SendLog), ld);
        }

        public void Info(string category,string msg)
        {
            LogData ld = new LogData();
            ld.Message = msg;
            ld.Level = "info";
            ld.Category = category;
            ld.Time = DateTime.Now.ToString("yyyyMMddHHmmssffff");
            ThreadPool.QueueUserWorkItem(new WaitCallback(SendLog), ld);
        }

        private void SendLog(object logData)
        {

            LogData ld = (LogData)logData;
            PutAttributesRequest request = new PutAttributesRequest()
                        .WithDomainName("fis-log")
                        .WithItemName(System.Guid.NewGuid().ToString())
                        .WithAttribute(new ReplaceableAttribute[]{new ReplaceableAttribute()
                            .WithName("msg")
                            .WithValue(ld.Message),new ReplaceableAttribute()
                            .WithName("level")
                            .WithValue(ld.Level),new ReplaceableAttribute()
                            .WithName("category")
                            .WithValue(ld.Category),new ReplaceableAttribute()
                            .WithName("time")
                            .WithValue(ld.Time)});
            lock (sdb)
            {
                sdb.PutAttributes(request);
            }

        }
    }
```