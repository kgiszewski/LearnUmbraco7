#LogHelper#

The `LogHelper` will send logging messages to your websites' log files located at `~/App_Data/Logs`.  The log is underpinned by `Log4Net` which is a popular logging framework from the Apache Foundation.  You can find more details about Log4Net here: https://logging.apache.org/log4net/.

##Log4net Config##
You can send a message to the log from any class.  There are different *levels* of logging you can use to go to the log.  Those levels are:

* Warn
* Debug
* Info
* Error

You can suppress errors from hitting the log based on level by altering the settings in your `~/config/log4net.config`.  You can fiddle with the formatting and even add additional levels.

##Logging Sample##
The `LogHelper` takes a class name as it's argument and will appear in the written log to assist where the error is coming from.  When logger with the `Error` level, you will send the exception class as well.

```
using System;
using Umbraco.Core.Logging;

namespace MyNamespace
{
    public class MyClass
    {
        public void MyMethod()
        {
            LogHelper.Info<MyClass>("Hello world!");

            try
            {
                //oh no an error!
            }
            catch (Exception ex)
            {
                LogHelper.Error<Exception>(ex.Message, ex);
            }
        }
    }
}
```

>Note: By default un-handled errors are sent to the log for you.

[<Back 11 - IOHelper.md](11 - IOHelper.md.md)

[Next> 13 - File Service](13 - File Service.md)