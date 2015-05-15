title: Accessing same App.config from different dll/assembly/exe
id: 18
categories:
  - code snippets
date: 2012-04-13 08:58:53
tags:
---

The built in ConfigurationManager/appSetting thing in .Net Framework seems to be powerful, but complicated.
I have a project with several dlls and exes. all these dll files and exe files want to access one configuration file. I didn't find an easy way to work this out with ConfigurationManager.
So I just write a simple class to read & write config file across entire solution.
```
public class Config
    {
        private static Configuration config;

        static Config()
        {

        }

        public static string GetValue(string key)
        {
            if (config == null)
            {
                Assembly assm = Assembly.GetExecutingAssembly();
                string location = assm.Location;
                string assemlyPath = Path.GetDirectoryName(location);
                var configMap = new ExeConfigurationFileMap { ExeConfigFilename = assemlyPath + &quot;\\App.config&quot; };
                config = ConfigurationManager.OpenMappedExeConfiguration(configMap, ConfigurationUserLevel.None);
            }
            return config.AppSettings.Settings[key].Value;
        }
    }
```
Usage:
```
Console.WrieLine(Config.GetValue(&quot;key-name&quot;));
```