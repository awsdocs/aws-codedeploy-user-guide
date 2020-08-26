# Create a file for your Lambda function<a name="tutorial-lambda-sam-create-lambda-function"></a>

Create the file for the function you update and deploy later in this tutorial\.

**Note**  
 A Lambda function can use any runtime supported by AWS Lambda\. For more information, see [AWS Lambda runtimes](https://docs.aws.amazon.com/lambda/latest/dg/lambda-runtimes.html)\. 

**To create your Lambda function**

1.  Create a text file and save it as `myDateTimeFunction.js` in the `SAM-Tutorial` directory\. 

1.  Copy the following Node\.js code into `myDateTimeFunction.js`\. 

   ```
   'use strict';
       
       exports.handler = function(event, context, callback) {
       
         if (event.body) {
           event = JSON.parse(event.body);
         }
       
         var sc; // Status code
         var result = ""; // Response payload
       
         switch(event.option) {
           case "date": 
             switch(event.period) {
               case "yesterday":
                 result = setDateResult("yesterday");
                 sc = 200;
                 break;
               case "today":
                 result = setDateResult();
                 sc = 200;
                 break;
               case "tomorrow":
                 result = setDateResult("tomorrow");
                 sc = 200;
                 break;
               default:
                 result = {
                   "error": "Must specify 'yesterday', 'today', or 'tomorrow'."
                 };
                 sc = 400;
                 break;
             }
             break;
             
       /*      Later in this tutorial, you update this function by uncommenting 
               this section. The framework created by AWS SAM detects the update 
               and triggers a deployment by CodeDeploy. The deployment shifts 
               production traffic to the updated version of this function.
               
               case "time":
               var d = new Date();
               var h = d.getHours();
               var mi = d.getMinutes();
               var s = d.getSeconds();
       
               result = {
                 "hour": h,
                 "minute": mi,
                 "second": s
               };
               sc = 200;
               break;
       */
             default:
               result = {
                 "error": "Must specify 'date' or 'time'."
               };
               sc = 400;
             break;
         }
       
         const response = {
           statusCode: sc,
           headers: { "Content-type": "application/json" },
           body: JSON.stringify( result )
         };
       
         callback(null, response);
       
         function setDateResult(option) {
       
           var d = new Date(); // Today
           var mo; // Month
           var da; // Day
           var y; // Year
       
           switch(option) {
             case "yesterday":
               d.setDate(d.getDate() - 1);
               break;
             case "tomorrow":
               d.setDate(d.getDate() + 1);
             default:
              break;
           }
       
           mo = d.getMonth() + 1; // Months are zero offset (0-11)
           da = d.getDate();
           y = d.getFullYear();
       
           result = {
             "month": mo,
             "day": da,
             "year": y
           };
       
           return result;
         }
       };
   ```

The Lambda function returns the day, month, and year for yesterday, today, or tomorrow\. Later in this tutorial, you uncomment code that updates the function to return information about the day or time you specify \(for example, the day, month, and year, or the current hour, minute, and second\)\. The framework created by AWS SAM detects and deploys the updated version of the function\. 

**Note**  
 This Lambda function is also used in an AWS Cloud9 tutorial\. AWS Cloud9 is a cloud\-based integrated development environment\. For information about how to create, execute, update, and debug this function in AWS Cloud9, see [AWS Lambda tutorial for AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/tutorial-lambda.html)\. 