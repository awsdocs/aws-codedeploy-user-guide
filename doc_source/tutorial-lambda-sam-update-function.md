# Step 2: Update the Lambda function<a name="tutorial-lambda-sam-update-function"></a>

 In this topic, you update your `myDateTimeFunction.js` file\. In the next step, you use the file to deploy the updated function\. This triggers CodeDeploy to deploy it by shifting production traffic from the current version of the Lambda function to the updated version\. 

**To update your Lambda function**

1.  Open `myDateTimeFunction.js`\. 

1.  Remove the two comment markers \("`/*`" and "`*/`"\) and the explanatory text at the start and end of the `case` named `time` in the `switch` block\. 

    The uncommented code lets you pass a new parameter, `time`, to the function\. If you pass `time` to the updated function, it returns the current `hour`, `minute`, and `second`\. 

1.  Save `myDateTimeFunction.js`\. It should look like the following: 

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