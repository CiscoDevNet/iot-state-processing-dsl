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
To get started with the iot-state-processing-dsl language and writing your own rules, go to src/test/GettingStarted.md.  

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

## How to test your rule sets
We highly recommended testing your rule set before adding it to the product.

Use the **iot-state-processing-dsl-*version*-all.jar** tool to process a data sample using your draft rule set.

1. Prepare the data using format similar to the *.data in the tutorial.
1. Create your rule set.
1. Run the following command:
    **java –jar iot-state-processing-dsl-*version*-all.jar -h**
1. Review the resulting data to verify that the rule set worked as expected.
