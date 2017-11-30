# Creating rule sets for IoTDC data routing policies

Use the files in this tutorial to understand how rule sets can be used in IoTDC data policies to filter, throttle, or transform the data that is routed from a gateway to the destination(s). 
- You can create multiple rule sets for your organization, and rule sets can be assigned to multiple data policies.
- Rule sets can also include destinations where the data should be sent.  For example, if a sensor temperature exceeds 100º, send the data to a specific destination broker.

## Summary steps

To create rule sets:

1. Review the information below to understand:
   - What you can do with rule sets
   - The example rule files provided in this tutorial
   - How to test your rule sets
2. Use the example rule sets to learn about rule syntax. 
   - Review the comments and examples in each file to understand how to apply the rules and define destinations for your data.
3. Create your own rules by modifying a sample rule file or writing a new rule file.
4. Test the rule before using it in an IoTDC data policy. 
5. Add the rule to IoTDC using the admin console.  
   - Go to **Data > Data Rule Sets** and click **Add Rule Set**.
6. Create a data policy that uses the rule set. 
   - Go to **Data > Data Policies**.

## What you can do with rule sets
You can create rules sets that combine any of the following:

|Data|Description|Example|
|---------|---------|---------|
|Data threshold|When data crosses a threshold, send it to a destination|If the sensor temperature is greater that 100º, send the data to a destination broker.|
|Transform data| Data can be converted or changed| If a sensor sends temperature data in Celsius, convert it to Fahrenheit.|
|Throttling or Filtering|Limit or define how often data is sent|If the asset sends data every 5 seconds, only send it once every 5 minutes instead.|
|Combining Rules|These rules can be combined in a single rule set|If the temperature is below 60º, send data once every minute, or if the temperature is above 60º, send the data immediately.|
|Combining data|Combine the data from multiple assets|Additional conditions can be specified for the combined data. For example, if the engine temperature is greater than 100º, and the brake temperature is greater than 150º, and oil pressure is less that 40 PSI, then take an action. See the additional example below(**).|

**If a trucking company installs a gateway in a truck that has multiple assets (sensors), the rule sets can combine the data from each sensor into a single combined message. For example, the latest data from the following assets would be combined into a single message and sent to the destination.
- The engine sensor (asset) sends data to the gateway every 5 seconds.
- The brakes sensor sends data to the gateway every 1 second.

## File types in this tutorial
The attached rule set tutorial and examples provide a hands-on method to start creating your own rule sets. Each tutorial includes 3 types of files.

    *.rules — sample rule set with inline descriptions to help you understand the syntax. 
    *.data — file containing the actual data sent by a sensor, such as a temperature sensor in a truck. Use the file specified in the .rule example.
    *.output — this file is used for debugging so you can see the data generated using the above files, logs, and other information. 

For example, open a .rules file and review the comment lines that begin with "–" to understand the corresponding syntax. Run the rule against the *.data file specified in the .rule file to produce the resulting data in the *.output file.

## Learn how to write rules  
To get started with the iot-state-processing-dsl language and writing your own rules, go to [GettingStarted.md](GettingStarted.md).  

## Example rule sets included in this tutorial
The following rule sets are included in the tutorial:

|Example rule set|Description|
|---------|---------|
|complexPayload.rules|Shows how to handle arrays of data in observations|
|forUsage.rules|Shows how to create conditions on data over an interval|
|relationalExpression.rules|Shows how to compute expressions|
|sendUsage.rules|Shows how to send data|
|throttleUsage.rules|Shows how to throttle data|
|timeSeriesUsage.rules|Shows how to use time series|
|tranformationScenario.rules|Shows how to do data transformation|
|TSExperiment.rules|Shows proper and erroneous time series operations|
|variableInitLogAccessMsg.rules|	Shows how to initialize variables, create logging statements, and access data inside observations|
|whenUsage.rules|Shows the usage of conditionals|


# To RUN the build-in command line rule eval app :
```
usage: java -jar iot-state-processing-dsl-<version>-all.jar
 -a,--amqp-file <arg>   name of AMQP config file(absolute path)
 -d,--data-file <arg>   name of data file(absolute path)
 -h,--help              help on using the jar file
 -l,--log-level <arg>   loglevels: trace, debug, info, warn, error
 -o,--output <arg>      Optional output
 -r,--rule-file <arg>   name of rules file(absolute path)

Reading observation data from a file 
Usage:
java -jar build/libs/iot-state-processing-dsl-<version>-SNAPSHOT-all.jar -r tutorial/whenUsage.rules -d tutorial/genericData.data  -l warn -o tutorial/testOutput02

```

In this mode a command line option of "-d" is mentioned along with the file name. The data in the file is processesed against the rule set one time. "-d" option takes precedence over "-a" option when both are mentioned in the command line.

```
Reading observation data from AMQP brokers (RabbitMQ)
Usage:
java -jar build/libs/iot-state-processing-dsl-<version>-SNAPSHOT-all.jar -r tutorial/whenUsage.rules  -a amqp_config.ini -l DEBUG -o tutorial/testOutput02

```

This mode needs a AMQP configuration file which has details realted to amqp broker information like host/ports, exchanges, vhost, username/password, SSL details. The configuration file can mentioned with "-a" option. In this mode the application connects with the AMQP broker over SSL or non-SSL based on setting in the configuration file.

The application in this mode authenticates with the AMQP brokers, bind a queue to an exchange and starts consuming the messages. The consumed messages (one at a time) are processed against the rule set mentioned in the rules file (mentioned with -r option). When no data is available in the AMQP exchanges, the application waits (indefinately) for the messages to arrive. User can terminate the running application anytime (Eg: CTRL-C). The output is sent to a file when "-o" option is mentioned. Also it could be seen in the console (the amount of data logged to console depends upon the log level) 

The AMQP configuration allows the users to connect to the broker over SSL or non-SSL (the parameter which controls this being "ssl_connection" in config file). When the client specifies the connection over SSL, user can use the default settings mentioned in the file related to SSL version and certificate. The CA certificate mentioned here could be used to validate the certificate from IOTDC AMQP brokers. Users can also mention their own CA certificate in the configuration file when connecting with the brokers not part of IOTDC. 

```
The AMQP broker configuration file is used by the application to establish communication and read data from the AMQP brokers. The format used is the "ini" file format. 
The section "[rabbitmq]" is used to specify the details. The individual configuration element details are listed below:
[All the fields are mandatory unless mentioned]

host=
The above parameter is the hostname of the AMQP broker.

port=
The above parameter is the port on which the client services are available.

vhost=
The above parameter is the "vhost" of the AMQP broker from which the data is to be read.

exchange_name=
The above parameter is the exchange on the above vhost in AMQP broker from which the data is is to be read. 

exchange_type=
The above parameter is the type of exchange mentioned above. 

user_name=
The above parameter is the username which has access to read the data from above exchange. 

password=
The above parameter is the password which will be used along with the above username to authenticate/authorize the user before reading the data. 

ssl_connection=
The above parameter is to use SSL or non-SSL based communication between application and AMQP broker. 

ssl_version=TLSv1.2
The above parameter is to TSL version that need to be used when SSL communication is mandated (Optional).

ssl_cert_path=./certificates/wild-ca.crt
The above parameter is to set the certificate path which will be used to validate the server certificate. (Optional. If not mentioned the default trust store will be used
trustStore : /Library/Java/JavaVirtualMachines/jdk1.8.0_73.jdk/Contents/Home/jre/lib/security/cacerts
trustStore type : jks

A sample configuration file is provided here.
 
```

