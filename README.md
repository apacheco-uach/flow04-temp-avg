# flow04-temp-avg
Each second generate temp value. Computes temp average every 5 seconds.

Ejercicio libre (26/01/2022 && 02/02/2022).

Source: http://noderedguide.com/node-red-lecture-6-example-6-3-using-context-to-generate-rolling-averages/

## Dashboard:
![flow4 screen](https://user-images.githubusercontent.com/95945745/152285564-1e747c2e-af5b-4277-92d3-382a68571edd.jpg)

## Flow:
![flow4 nodes](https://user-images.githubusercontent.com/95945745/152285607-c21bbc77-c062-4983-b2fa-2c7fbbe3ea2b.jpg)

## Inject node configuration:
![flow4 config](https://user-images.githubusercontent.com/95945745/152286424-e3437da1-a3d9-415e-8c53-bd5bb3057780.jpg)

## Function: ramp
```javascript
if (!context.value) {
  context.value = 0;
}
msg.payload = {
  value:context.value
}
context.value +=1;
if (context.value > 9) {
  context.value = 0;
}
return msg;
```

## Function: average
```javascript
var currentTime = new Date().getTime();

if (!context.lastTime) {
    context.lastTime = currentTime;
    context.sum = msg.payload.value;
    context.count = 1;
}
if (currentTime-context.lastTime > 5000) {
    // calculate average for previous messages
    msg.payload.average = context.sum/context.count;
    // start tracking average again
    context.sum = msg.payload.value;
    context.count = 1;
    context.lastTime = currentTime;
    msg.payload.count = context.count;
} else {
    context.sum += msg.payload.value;
    context.count += 1;
    msg.payload.average = "*"
    msg.payload.count = context.count;
}
return msg;
```
