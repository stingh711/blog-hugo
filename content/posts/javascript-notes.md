+++
title = "Javascript notes"
date = 2018-06-16
tags = ["programming", "javascript"]
type = "post"
draft = false
+++

Just some notes on some popular javascript libraries.


## bootstrap-daterangepicker {#bootstrap-daterangepicker}


### How to get startDate and endDate {#how-to-get-startdate-and-enddate}

```javascript
     var range = $("#range").daterangepicker();
     var start = range.data("daterangepicker").startDate;
     var end = range.data("daterangepicker").endDate;
```


## lodash {#lodash}


### map {#map}

```javascript
     var data = [{label: '4/1', value: 10}, {label: '4/2', value: 14}];
     var labels = _.map(data, function(item) { return item.label; });
     var values = _.map(data, function(item) { return item.value; });
```

It is especially useful when preparing data for some chart libraries.


### convert array to object {#convert-array-to-object}

```javascript
     var data = [{value: 0, name: 'Working'}, {value: 1, name: 'Idle'}];
     var result = _.chain(data).keyBy('value').mapValues('name').value();
     //The result will be {0: 'Working', 1: 'Idle'}
```


## jquery {#jquery}


### How to pass array when using $.get {#how-to-pass-array-when-using-dot-get}

```javascript
     $.get("/api", {"name[]": ["George", "Jacky"]}, function(data) {});
```

If you are using Spring MVC for the server side, here is the relative code to receive multiple parameters.

```java
     @GetMapping("/api")
     @ResponseBody
     public YourResponseEntity api(@RequestParam("name") String[] names) {
     //deal with names
     }
```


## es6 {#es6}


### How to get keys of an object in javascript {#how-to-get-keys-of-an-object-in-javascript}

```javascript
   const data = {name: "sting", age: 10};
   Object.keys(data);
```