---
title: "Typescript Message Mapping Strategy"
date: 2019-02-09T11:26:55Z
draft: true
---

Hello!

A common pattern in event driven applications is to connect messaging systems together. For example we may have multiple events being received from different systems that have different structure that we want to map to a common format to process.

![Message Mapping](/static/message_mapping.png)

Let's say we had a garage that specialises in repairs - there is already a notification system in place that accepts messages in this format:

```
{
 "event": {
      "traceId": "unique correlation id",
      "type": "com.myrepairs.request.1.0.0",
      "timestamp": "2019-02-01T09:06:21Z"
    },
    "car": {
        "make": "",
        "model": "",
    },
    "customer": {
        "name": "",
        "phone": ""
    },
    "notes": ""
}
```


### Request Event from Super Cars

We have a contract with Super Cars to repair their customers cars, we can subscribe to their message queue and they send notifications in this format:

```
{
    "event_type1": {
        "correlationId": "",
        "type": "super.cars.event",
        "make": "VW",
        "model": "Golf",
        "details": "flat tire",
        "customerDetails": {
            "name": "Mr Jones",
            "contactNumber": "0123456"
        }
    }
}
```

### Request Event from Bobs Garage

Bob's Garage also want to send their repairs to our garage, similiar to Super Cars event, they convey the problems with the car and contact details but no correlation Id and different fields for contact:

```
{
    "event_type2": {
        "type": "bobs.garage.car.event",
        "timestamp": "2019-02-01T09:06:21Z",
        "carMake": "VW",
        "carModel": "Golf",
        "Contact": {
            "firstName": "Sideshow",
            "lastName": "Bob",
            "phone": "121321637"
        }
    }
}
```

## A strategy to handle mapping different messages


An interface to 

```
interface IVehicleMappingStrategy {
  isMatch(message: any): boolean;
  map(message: any): any;
}
```

```
class CarMappinggStrategy implements IVehicleMappingStrategy {
    isMatch(event: any): boolean {

    }


}
```